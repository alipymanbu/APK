# DroidCam OBS 直播与录屏连接方法（插件版）

> 不想走虚拟摄像头、想把手机直接变成 OBS Studio 里的一个视频源？这一篇讲官方 DroidCam OBS 插件的安装、连接、4K 隐藏开关与多机位玩法，也是 Mac 用户唯一可用的连接路线。
> **相关文档**：[手机当电脑摄像头使用教程.md](手机当电脑摄像头使用教程.md) · [画面卡顿与画质提升方法.md](画面卡顿与画质提升方法.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **DroidCam 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/15f94ef1ab1e](https://pan.quark.cn/s/15f94ef1ab1e)

---

## 一、插件路线与客户端路线差在哪

| | 客户端路线（见 [使用教程](手机当电脑摄像头使用教程.md)） | OBS 插件路线（本篇） |
| --- | --- | --- |
| 电脑端装什么 | DroidCam 客户端（注册虚拟摄像头） | OBS 插件（不装客户端） |
| 手机端用哪个 App | 经典版 DroidCam（网盘里这份） | 独立的 **DroidCam OBS** App（应用商店另装） |
| OBS 里怎么用 | 添加「视频采集设备」选 DroidCam Webcam | 直接添加 DroidCam 源 |
| 系统支持 | Windows / Linux | Windows / Linux / macOS |
| 同时接手机数 | 最多 3 台 | 不限（每台手机加一个源） |
| 最高画质 | 1080p | 4K（隐藏开关，见第四节） |

macOS 没有官方客户端，想在 Mac 上用手机摄像头，插件路线是唯一解。

## 二、安装插件与对应的手机 App

两条路线的手机端 **不是同一个 App**：插件路线要在手机上另装应用商店里的「DroidCam OBS」；网盘里这份经典版 6.28 只配套电脑客户端，装它走不了插件路线。电脑端插件的安装：

- **Windows**：官方页 [droidcam.app/obs](https://www.droidcam.app/obs/) 下载安装程序（写作时为 v2.5.1，要求 OBS Studio v32 及以上，x64 与 arm64 都有）。
- **macOS**：同一页面按你的 OBS 版本下载对应 pkg；安装时把「Install Location」设为你的 Home 文件夹。
- **Linux**：`flatpak install flathub com.obsproject.Studio.Plugin.DroidCam`（要求 Flathub 版 OBS Studio）。

装完**重启 OBS Studio** 再继续。

## 三、连接手机

### WiFi（与客户端一样的准备：同一网络）

1. 手机打开 App，停在显示 WiFi IP 的首页。
2. OBS 里「来源」→ 添加 **DroidCam** 源，打开属性。
3. Device 下拉选 **Use WiFi**，在 WiFi IP 栏填手机上显示的 IP（找不到输入框就把属性面板往下滚）。
4. 点 **Activate** 连接；停止或改设置用 **Deactivate**。

嫌手填麻烦就点 **Refresh Device List** 自动发现（条件：手机 App 开着、路由器放行多播）。找不到设备时多点几次刷新，实在不行手填 IP。**Mac 用户**先去「系统设置 → 隐私与安全性 → 本地网络」允许 OBS 访问，否则发现不了设备。

### USB（Android）

1. 手机开 USB 调试（步骤见 [手机当电脑摄像头使用教程.md](手机当电脑摄像头使用教程.md) 第三节）。
2. 插线后，在 DroidCam 源属性里点 **Refresh Device List**，手机上弹「允许 USB 调试」就点允许。
3. Device 下拉里选你的手机（不再是 Use WiFi），点 **Activate**。

驱动方面 Windows 一般自动装；**Mac 无需驱动**；**Linux 要先装 adb**：Debian/Ubuntu `sudo apt install adb`，Arch `sudo pacman -S android-tools`，Fedora/SUSE `sudo yum install android-tools`。认不到设备时，把手机 USB 模式从 MTP 切成「仅充电」或 PTP 再刷新。

## 四、4K 隐藏开关

4K 是插件版独有的能力，但默认藏起来，要手动激活：

1. 确认三件套版本：OBS Studio 29.1+、DroidCam OBS 插件 2.2+、手机端 DroidCam OBS App 4.0+（写作时安装最新版都满足）。
2. 在 DroidCam 源属性里，Device 选 **Use WIFI IP**，然后在 WiFi IP 输入框里**填字符 `4k`**（不是真 IP），点 Activate，出现确认消息后关掉并 OK 保存。
3. 重新打开这个源的属性，Resolution 下拉会多出一整排选项，最高到 3840x2160（UHD）；同时勾上 **Allow hardware acceleration**。
4. 插件 2.4.0 起（需 DroidCam OBS App v8.x）支持在 Resolution 栏直接输自定义数值；手机 App 设置里的 **Camera Information** 能列出你这台机器支持的采集分辨率。

开之前想清楚值不值：4K 的数据量是 1080p 的 4 倍，手机针对「录 4K 视频存本地」的优化对「实时传输」并不生效，负载明显更大；而直播平台和会议软件大多只收 720p/1080p，4K 输入再缩到 1080p 输出收益甚微。**录屏要保留细节、或打算在 OBS 里后期裁切放大**的场景才值得开。

## 五、多机位与多场景

- **多台手机**：每台手机在 OBS 里各加一个 DroidCam 源，WiFi、USB 可以混用。
- **同一台手机用在多个场景**：把同一个 DroidCam 源加到多个场景即可；若多个源共用同一台手机，勾上源的 **Deactivate when not showing**。
- **同一画面套不同滤镜**：用 OBS 社区的 Source Clone 插件复制出多份源，各自挂滤镜。
- 插件还提供通过 OBS 的 Custom Browser Dock 加载的遥控界面，手机上就能控制录制，具体入口以官方 OBS 页当时显示为准。

## 六、StreamLabs 用户怎么办

StreamLabs Desktop 不支持外部插件，两条替代路：

1. 照 [使用教程](手机当电脑摄像头使用教程.md) 装客户端，把手机当普通摄像头，在 StreamLabs 里选 DroidCam Webcam。
2. 加一个**浏览器源**，URL 填 `http://手机IP:端口/video/1280x720`（尺寸可换成 640x480 或 1920x1080，端口以手机 App 显示为准）。

插件装好后连接异常时，先按 [连接失败与常见报错排查.md](连接失败与常见报错排查.md) 过一遍；画面流畅度调优见 [画面卡顿与画质提升方法.md](画面卡顿与画质提升方法.md)。
