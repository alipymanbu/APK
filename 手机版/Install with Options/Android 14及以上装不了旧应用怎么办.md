# Install with Options：Android 14 及以上装不了旧应用怎么办

> 老应用装到 Android 14/15/16 上报「应用未安装」或 `INSTALL_FAILED_DEPRECATED_SDK_VERSION` —— 这是系统按目标版本拦人，这篇讲清规则线与绕过办法。
> **相关文档**：[安装失败错误代码解决办法.md](安装失败错误代码解决办法.md) · [高级安装选项怎么用.md](高级安装选项怎么用.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **Install with Options 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/5fbe0c775706](https://pan.quark.cn/s/5fbe0c775706)

---

## 一、这不是文件坏了，是系统在按「目标版本」拦人

你下好了一个多年前的老应用，APK 完整、签名正常，点安装却只回一句「应用未安装」，或者在 Install with Options 的结果里看到：

```
INSTALL_FAILED_DEPRECATED_SDK_VERSION: App package must target at least SDK version 24, but found 22
```

原因是 Android 从 14 开始加了一条**系统级安装拦截**：应用声明的目标 API（`targetSdkVersion`）低于当版系统的下限时，无论从哪装（文件管理器、商店、 adb）都直接拒绝。它拦的不是「旧应用」本身，是「目标版本太老、按老安全模型运行」的应用 —— 恶意软件常躲在这类老目标版本里绕过现代权限模型，所以 Google 把门槛逐年抬高。

**先确认你撞的是这条**：`INSTALL_FAILED_DEPRECATED_SDK_VERSION` 或「应用未安装」+ 应用本身很老 → 是本篇的问题；签名不匹配、降级失败、CPU 架构不符各有各的报错，见 [安装失败错误代码解决办法.md](安装失败错误代码解决办法.md)。

## 二、每个 Android 版本的门槛线

| 系统版本 | 新安装要求的最低 targetSdk | 对应的应用大致年代 |
| --- | --- | --- |
| Android 14 | 23 | 目标 Android 6.0 及以上的应用才能装 |
| Android 15 | 24 | 目标 Android 7.0 及以上的应用才能装 |
| Android 16 | 24（未再抬） | 同 Android 15 |

（门槛线依据官方文档与公开整理，截至 2026-03；更高版本是否继续上调，以 [Android 开发者文档](https://developer.android.google.cn/about/versions) 当时显示为准。）

两条容易搞错的边界：

- **已装的应用不受影响**：升级系统不会把你装好的老应用清掉，这条拦截只作用于**新安装**。
- **目标版本低 ≠ 装不上运行不了**：`minSdk` 够就能跑，拦你的只是 target 这一项。

另外 Android 10 起，启动目标版本很低（API 22 及以下）的应用时系统还会弹一次警告 —— 那是运行时提醒，不是安装拦截，别混为一谈。

## 三、怎么装上去：用壳参数绕过

这条拦截留了一个 shell 级的绕过参数，电脑侧 adb 的写法是：

```bash
adb install --bypass-low-target-sdk-block 应用名.apk
```

**你不需要为此连电脑** —— Install with Options 的「Bypass Low Target SDK Block（绕过低目标 SDK 限制）」选项，做的就是把同一个参数交给 Shizuku 的 shell 去执行（选项只在 Android 14+ 的系统上出现，因为门槛是 14 才有的）。操作步骤：

1. 按 [下载与安装教程.md](下载与安装教程.md) 装好 IWO 并让 Shizuku 正常运行；
2. 选中那个老应用的 APK；
3. 勾上「Bypass Low Target SDK Block」；
4. 点安装。其余错误码照 [安装失败错误代码解决办法.md](安装失败错误代码解决办法.md) 排查。

选项的适用边界也按 [高级安装选项怎么用.md](高级安装选项怎么用.md) 那张表来：它只解「目标版本太老」这一种拦，签名、降级、ABI 的问题它一概不管。

## 四、绕不过去的两种情况

- **测试构建之外、系统版本太老**：这个选项是 Android 14 才引入的，Android 13 及以下的系统根本没有这道拦，你也看不到该选项 —— 老系统上装不上是别的原因。
- **目标版本老到离谱且设备策略收紧**：受管设备（企业配置文件）里，管理员可能连 shell 绕过一并禁掉，这种只能找管理员。

实在装不上时的兜底顺序：先确认 APK 文件完整、剩余存储空间够，再核对是不是签名或降级问题（[安装失败错误代码解决办法.md](安装失败错误代码解决办法.md)），最后才回到本篇的门槛线核对 target 版本。
