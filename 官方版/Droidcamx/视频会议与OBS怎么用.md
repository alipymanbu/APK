# DroidCamX 视频会议与OBS怎么用

> 本篇讲连接成功之后怎么让 Zoom、Teams、Discord、OBS 用上手机画面。还没连接的先看[手机连接电脑方法](手机连接电脑方法.md)。
> **相关文档**：[手机连接电脑方法](手机连接电脑方法.md) · [下载与安装教程](下载与安装教程.md) · [画面卡顿与画质优化](画面卡顿与画质优化.md)

---

> [!IMPORTANT]
> **DroidCamX 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7548e738eed4](https://pan.quark.cn/s/7548e738eed4)

---

## 一、它就是一个普通摄像头

客户端连接成功后，系统里会多出一个名为 **DroidCam Video** 的摄像头设备。对 Zoom、Teams、Discord、Skype 来说，它和插了一个 USB 摄像头没有区别：

1. 打开会议软件的设置，找到视频/摄像头选项。
2. 在设备列表里选 **DroidCam Video**。
3. 如果列表里没有，把会议软件**完全退出再重开**——多数软件只在启动时枚举摄像头。
4. 想连手机麦克风的话，客户端添加设备时勾选 Enable Audio，软件里再选 DroidCam 对应的音频输入。官方的建议是：电脑本身有麦克风就别连音频，音质和延迟通常不如你电脑现有的设备。

## 二、选了摄像头但显示绿屏

典型的初始化顺序问题，两步处理：

1. 在 DroidCam 客户端里 File > Exit 完全退出，重新启动客户端。
2. 还不行就重启电脑，让虚拟摄像头重新注册一次。

## 三、OBS 直播的两条路

**第一条：走经典客户端（装了 DroidCam Client 的电脑）。** OBS 里添加「视频采集设备」源，设备选 DroidCam Video，和普通摄像头用法一致。配合画质设置（720p/1080p）可以满足常规直播需求，调优见[画面卡顿与画质优化](画面卡顿与画质优化.md)。

**第二条：官方的 DroidCam OBS 插件。** 官方另有一套「DroidCam OBS 插件 + 手机端 DroidCam OBS 应用」的组合，直接作为 OBS 源接入，不需要装经典客户端，支持 Windows / Linux / macOS，最高可到 4K。要注意两点：

- 手机端 DroidCam OBS 和你手上这份 DroidCamX 是**两个独立的应用**，专业版授权互不通用，需要的话在 OBS 插件体系里另配。
- 插件版本和 OBS 版本有对应关系（截至撰写时要求 OBS v32 及以上），以官方页面 [droidcam.app/obs](https://www.dev47apps.com/obs/) 为准。

多台手机在 OBS 里就是多加几个 DroidCam 源；同一台手机要出现在多个场景，把同一个源加到多个场景即可。

## 四、StreamLabs 的接法

StreamLabs Desktop 不支持外部插件，所以既用不了 OBS 插件，也走不了采集卡那种思路。可用的是**浏览器源**方式：手机端开起来后，在 StreamLabs 里添加 Browser 源，URL 按这个模板填：

```text
http://手机IP:4747/video/1280x720
```

合法尺寸只有 640x480、1280x720、1920x1080 三档，按需替换。
