# Animeko iPhone和iPad怎么装

> 讲清 iPhone、iPad 上为什么不能直接装网盘里那份包，官方 IPA 要用什么工具装，以及免费签名带来的两个现实限制。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [注册登录与Bangumi账号同步.md](注册登录与Bangumi账号同步.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **Animeko 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f581b37ef550](https://pan.quark.cn/s/f581b37ef550)

---

**网盘里那份是安卓 APK，iPhone 和 iPad 装不了。** iOS 走的是另一套流程：下载官方的 IPA 文件，再用「侧载」工具借助 Apple 的开发者签名装进设备。这一篇把两种常用装法和免费签名的限制讲清楚。

## 一、先知道两个免费签名的限制

用普通 Apple ID（没买开发者会员）侧载应用，有两条绕不过的限制：

| 限制 | 影响 | 应对 |
| --- | --- | --- |
| 签名有效期 7 天 | 到期后应用打不开 | 每 7 天续签一次（工具支持连同一 Wi-Fi 自动续签） |
| 单个 Apple ID 最多同时装 3 个侧载应用 | 装第四个会失败 | 想加新的就先删一个旧的 |

这两条是 Apple 开发者签名机制本身的规则，不是应用的问题。

## 二、准备工作

- **硬件**：一台 macOS 或 Windows 电脑 + 一台 iPhone / iPad + 一根稳定的数据线；
- **账号**：一个用邮箱注册的 Apple ID；
- **安装包**：Animeko 的 IPA 文件，从项目的 GitHub Release 页取（`https://github.com/open-ani/animeko/releases`）；
- **Windows 用户额外要装 iTunes**，且必须是**非 Microsoft Store 版本** —— 装了商店版的要先卸载，再从官网下载安装。

## 三、方法一：用 Sideloadly 装

1. 按你的系统从官网下载 Sideloadly（Windows 通常选 64 位；不确定就按 `Win` + `R` 输入 `msinfo32`，看「系统类型」）；
2. 打开工具，在 `Apple ID` 栏填 Apple 账号邮箱；
3. 用数据线连上手机。手机上若弹出「要信任此电脑吗？」，点**信任**并输入锁屏密码；连接成功后设备名会出现在 `iDevice` 栏；
4. 点左侧 IPA 图标，选中下载好的 Animeko IPA，点 `Start`；
5. 首次使用要输入 Apple ID 密码；开了双重认证的话，填手机上收到的 6 位验证码；
6. 进度条跑完显示 `Done.` 即安装成功。

`Start` 左侧的「刷新」开关**建议保持开启**（默认就是开），这样设备在同一 Wi-Fi 下可以每 7 天自动续签，不必每次手动重装。

## 四、方法二：用 AltStore 装

步骤比 Sideloadly 多，但后续更新更方便。

1. **Windows 用户**额外安装 iCloud for Windows，同样必须用**非 Microsoft Store 版本**；
2. 下载并运行 AltServer（电脑端）：macOS 解压后把 `AltServer.app` 拖进「应用程序」，图标在菜单栏；Windows 解压后运行安装程序，**以管理员身份运行**，图标在系统托盘；
3. 连接设备并**开启 Wi-Fi 同步**（自动续签要用）：macOS 在访达侧边栏勾选「在 Wi-Fi 下显示此 iPhone/iPad」；Windows 在 iTunes 的设备摘要里勾选「通过 Wi-Fi 与此 iPhone/iPad 同步」；
4. 点电脑端的 AltServer 图标，选「Install AltStore」并选择你的设备，输入 Apple ID 与密码，等电脑弹出「AltStore 已安装到设备」；
5. **iOS 16 及以上**还必须开启开发者模式：`设置 → 隐私与安全性`，拉到最底部找到「开发者模式」并打开，按提示重启设备确认；
6. 安装 Animeko：按住键盘的 `Option`（macOS）或 `Shift`（Windows）点 AltServer 图标，选「Sideload .ipa…」，选中 IPA 并输入账号密码。

也可以把 IPA 传到手机上，在 AltStore 的 `My Apps` 页点左上角 `+` 直接安装。

## 五、首次打开提示「不受信任的开发者」

这是侧载应用第一次启动时的常见提示，按下面放行：

1. 打开 iOS 的 `设置 → 通用`；
2. 向下找到 `VPN 与设备管理`（旧系统里可能叫「描述文件与设备管理」）；
3. 在「开发者 APP」一栏点你的 Apple ID；
4. 点「信任 [你的邮箱]」并确认，之后就能正常打开。

## 六、装上之后的几点

- **进度和收藏照样能同步** —— 登录同一个 Bangumi 账号即可，iOS 版与安卓版是用同一套账号体系，见 [注册登录与Bangumi账号同步.md](注册登录与Bangumi账号同步.md)；
- **缓存是本机的**：卸掉重装、换设备都要重新缓存，换机前先想清楚哪些还下得动，见 [离线缓存与没网时怎么看.md](离线缓存与没网时怎么看.md)；
- **早期 iOS 版有过「缓存无法播放」的已知问题**，遇到先更新版本再判断是不是缓存坏了。
