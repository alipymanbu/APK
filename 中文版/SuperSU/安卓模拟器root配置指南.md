# 安卓模拟器 root 配置指南（BlueStacks / 官方 AVD）

> 本篇讲在电脑上的安卓模拟器里配好 root 环境并装上 SuperSU：BlueStacks 4 借第三方工具解锁，官方模拟器（Android Studio 的 AVD）走自带 root 通道，两条路的步骤与坑分开说。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与报错处理.md](常见问题与报错处理.md) · [root权限授权与日常管理.md](root权限授权与日常管理.md)

---

> [!IMPORTANT]
> **SuperSU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ae57de1aba73](https://pan.quark.cn/s/ae57de1aba73)

---

## 一、先明确：模拟器和真机是两套逻辑

模拟器没有 recovery 模式，真机那套"TWRP 卡刷 ZIP"在这里走不通；反过来，模拟器搞坏了也无所谓——关掉重开就回到初始状态，零风险。所以模拟器上的思路是：先拿到 root 通道，再装 SuperSU 当授权管理器。

| 你用的模拟器 | 路线 |
| --- | --- |
| BlueStacks 4 及更早版本 | 第三方工具（BlueStacks Tweaker）解锁后装 SuperSU |
| Android Studio 官方模拟器（AVD） | 选对镜像用自带 root 通道，再装 SuperSU |

## 二、BlueStacks 4：Tweaker 解锁，再装 SuperSU

社区教程给出的口径一致，步骤如下（工具是 XDA 开发者 Anatoly79 出的 BlueStacks Tweaker，第三方免费工具）：

1. 装好 BlueStacks 4，**先别启动它**；下载并解压 BlueStacks Tweaker；
2. 打开 Tweaker，Main 标签点 **Force Kill BS**，确保 BlueStacks 的所有进程都关干净（不放心就再开任务管理器看一眼）；
3. 切到 **Root** 标签，点 **Unlock**，等底部状态栏出现 **Unlock: True**（右上角 ADB 和 BlueStacks 两个指示灯变绿）；
4. 回 Main 标签点 **Start BS** 启动模拟器，等它完全加载好；
5. 再回 Root 标签点 **Patch**，提示完成后 BlueStacks 的 root 通道就绪；
6. 装管理器：从开头的 [SuperSU 安装文件资源（夸克网盘）](https://pan.quark.cn/s/ae57de1aba73) 拿 APK 装进模拟器（Tweaker 菜单里那个 Install SuperSU 按钮装的是 2.79 旧版，要 2.82 就自己装 APK）；
7. 打开 SuperSU，提示更新 SU 二进制 → 点「继续」→ 选 **Normal**；
8. 它要求"重启"时，模拟器内重启经常无效，直接把 BlueStacks 程序整个关掉再打开，然后用 root 检测类工具验证（可选）。

**时效提醒（截至 2026 年）**：Tweaker 作者已停止更新，这套流程对应 BlueStacks 4 及更早版本；BlueStacks 5 时代的社区方案普遍改用 Magisk 模板，没有把 SuperSU 装进 BlueStacks 5 的稳妥现成路线。想在模拟器里用 SuperSU，就装 BlueStacks 4。

## 三、官方模拟器 AVD：选对镜像是成败关键

Android Studio 自带的模拟器镜像分两种：带 **Google APIs** 的和带 **Google Play** 的。root 通道只在 Google APIs 镜像里开放——这是后面所有报错的根源，先选对：

- 创建 AVD 时，System Image 选 **Google APIs** 那一行（不要选 Google Play 图标那行）；
- 选错了的典型报错：`adb root` 提示 `adbd cannot run as root in production builds`。没救，重建一个 AVD 换镜像。

镜像对了，按顺序执行（电脑上要有 adb）：

```bash
adb root                      # 让 adbd 以 root 跑
adb remount                    # 让 /system 可写
adb shell
setenforce 0                   # 关闭 SELinux 强制模式
su --install                   # 安装 su
su --daemon &                  # 启动 su 守护进程
exit
adb install SuperSU_v2.82.1.apk   # 装管理器
```

装完打开 SuperSU，如果它提示更新二进制，照常「继续」→ **Normal**。不同镜像版本细节略有出入，以上是社区教程里反复出现的通用口径。

如果镜像里没有现成的 su（执行 `su --install` 报找不到命令），还有一条进阶路：从 SuperSU 的卡刷 ZIP 里取出对应架构的 su 二进制（Android 5.0 及以上用 `su.pie`），手动放进 `/system/xbin` 并给 6755 权限。这步对路径和权限很挑剔，动手前先给 AVD 做个快照（AVD 自带），坏了能一键还原。

## 四、装完之后

模拟器里的 SuperSU 和真机上行为一致：授权弹窗、日志、默认策略都在。日常怎么用见 [root权限授权与日常管理.md](root权限授权与日常管理.md)；碰上"没有 SU 二进制""二进制更新失败"这类问题，先翻 [常见问题与报错处理.md](常见问题与报错处理.md)——模拟器场景的成因和真机略有不同，那篇里标出来了。
