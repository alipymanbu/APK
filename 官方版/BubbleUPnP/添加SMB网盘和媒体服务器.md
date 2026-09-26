# BubbleUPnP 添加SMB网盘和媒体服务器

> 本篇讲内容从哪来：电脑共享（SMB）、网盘、WebDAV、TIDAL/Qobuz、局域网媒体服务器怎么接进来。
> **相关文档**：[投屏到电视和Chromecast教程.md](投屏到电视和Chromecast教程.md) · [下载与安装教程.md](下载与安装教程.md) · [免费版和付费版有什么区别.md](免费版和付费版有什么区别.md)

---

> [!IMPORTANT]
> **BubbleUPnP 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/118936c1ffd6](https://pan.quark.cn/s/118936c1ffd6)

---

## 一、媒体库（Library）是入口

所有内容源都挂在「媒体库（Library）」页下：左上角的切换入口列出你能选的库——`Local and Cloud`（手机本地 + 网盘 + 各类挂载）、局域网的 UPnP/DLNA 媒体服务器、以及通过 BubbleUPnP Server 暴露的远程库。选中一个库之后按文件夹浏览，点条目播放（行为见 [投屏到电视和Chromecast教程.md](投屏到电视和Chromecast教程.md) 第三节）。

`Local and Cloud` 库内部按类型分成若干根文件夹，下面逐个说怎么把源接进来。添加服务器类的入口一般在库选择列表或 `More > 齿轮` 下的对应设置项里，不同版本措辞略有差异，按界面上的「添加服务器 / Add server」找即可。

## 二、SMB：读电脑和 NAS 上的共享

1. 确保手机和电脑在同一个局域网，电脑上已开文件共享（Windows 的「网络发现 + 文件和打印机共享」，或 macOS 的「文件共享」，或任意 Samba 服务器）。
2. 在 BubbleUPnP 里添加 SMB 服务器，填电脑的局域网 IP 或主机名、用户名、密码。
3. 连上后共享文件夹会以根目录出现，直接浏览播放；需要时可以在 SMB 对话框里勾选隐藏文件的显示。

坑点：个别路由器（如部分 Fritz!Box）和 NAS 对 SMB 浏览有兼容问题，旧版本曾修过这类问题——**先升级到最新版再试**；共享的用户名密码改动后，记得回应用里改掉存的凭据。

## 三、网盘：Dropbox / Box / OneDrive

在 `Local and Cloud` 里添加云服务，走一次 OAuth 授权即可浏览网盘里的音乐视频。播放云端内容有两种去向：

- **投给本机**：手机直接拉流播；
- **投给外部设备**：电视/Chromecast 自己去取流（Chromecast 是直接从云端拉的），手机可以退到后台。

Dropbox 曾出现过「进入文件夹报错」的问题，那是访问令牌续期的 bug，官方已修——**遇到就升级版本**，不要反复重装。

## 四、WebDAV：Nextcloud / ownCloud / 自建

添加 WebDAV 源填服务器地址与账号即可。注意两点：

- 走公网的 WebDAV 若是 http（非 https），应用会弹安全提醒，确认自己能接受再继续；
- 支持从 WebDAV 直接下载到手机（旧版本修过下载失败的问题，卡住就先升级）。

## 五、TIDAL 与 Qobuz

两个音乐服务在库里直接登录账号使用：支持浏览、搜索、收藏，专辑曲目能加入播放列表推给外部设备播。用官方 TIDAL/Qobuz 应用把专辑或曲目「分享到 BubbleUPnP」也可以。收藏心形图标在「正在播放」页长按封面或点信息按钮那一带操作。这两项是付费订阅服务，订阅费另算，与 BubbleUPnP 自己的许可（见 [免费版和付费版有什么区别.md](免费版和付费版有什么区别.md)）无关。

## 六、局域网媒体服务器：Kodi / Plex / Jellyfin / NAS

手机只管当控制端，内容让服务器供：

| 你有的东西 | 能不能当源 |
| --- | --- |
| Kodi、JRiver、miniDLNA、Serviio、MinimServer、Asset UPnP | 可以，标准 UPnP/DLNA 服务器 |
| Plex、Jellyfin | 可以浏览播放（注意：Plex 经 UPnP 不下发外挂字幕，字幕要靠本地媒体源那套路径） |
| 群晖 / 威联通 / WD 等 NAS 自带媒体服务器 | 可以 |
| 部分路由器内置 DLNA 服务器 | 可以 |

这些服务器会自动出现在媒体库切换列表里，不用手动填地址（同网段且路由器没隔广播时）。**让电视也来浏览你手机的内容**是反方向用法：BubbleUPnP 自带 DLNA 媒体服务器功能，默认在局域网里广播，电视/PS 上选到它就能翻手机文件（PlayStation 具体怎么操作见 [投屏到电视和Chromecast教程.md](投屏到电视和Chromecast教程.md) 第六节）。

## 七、手机第二张 SD 卡要手动挂

应用默认只暴露主存储。插了第二张 SD 卡想让它的内容进媒体库（同时也会进对外广播的本地媒体服务器），去 `Settings > Local Media Server > Filesystem > Content`，在「Custom Mount Point 1」里填第二张卡的挂载点；之后在媒体库里选「本地媒体服务器」就能在根目录看到它。

## 八、其他应用分享进来

浏览器、文件管理器等任何支持「分享/发送」的安卓应用，都能把媒体文件或链接直接丢给 BubbleUPnP 播放或入队。播网络电台则是反向操作：在播放列表页菜单里 `Add Stream URL` 手填流地址。
