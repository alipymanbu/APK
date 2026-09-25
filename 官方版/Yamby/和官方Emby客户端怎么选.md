# 和官方Emby客户端怎么选

> Yamby 与官方 Emby for Android 在收费、功能、平台上的实际差异，按你的使用场景对号入座。
> **相关文档**：[Pro专业版功能对比.md](Pro专业版功能对比.md) · [连接Emby服务器教程.md](连接Emby服务器教程.md) · [播放器手势与字幕设置.md](播放器手势与字幕设置.md)

---

> [!IMPORTANT]
> **Yamby 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e6c2aeaf9ade](https://pan.quark.cn/s/e6c2aeaf9ade)

---

## 一、两者分别是什么

- **Emby for Android**：Emby 官方出品的手机客户端，商店页包名 `com.mb.android`，功能与服务器端同步最紧密；另有独立的 Android TV / Fire TV 官方客户端。
- **Yamby**：第三方独立开发者出品的 Emby 客户端，Material Design 3 界面，面向 Android 手机与平板（官方描述未提 TV 版）。

同一台 Emby 服务器，两边都能连，登录用的是同一套服务器账号。

## 二、收费：差别最大的地方

按 Emby 官方 Feature Matrix（截至 2026-09 以官方页面为准）：

| | 免费状态 | 完整播放 |
| --- | --- | --- |
| 官方 Emby for Android | 只能播 **1 分钟**（试播） | 需 **App Unlock**（应用内单独解锁）或服务器有 **Emby Premiere** |
| Yamby | 核心播放功能免费可用 | Pro 是 Yamby 自己的独立内购，与 Emby Premiere 是两码事 |

两点补充：

1. **Yamby 的 Pro 现状有两种说法并存**：有社区指南与开发者渠道公告称，因开发者 Google 账号问题，2025 年起已把绝大部分付费功能免费放出；也有用户反馈 Google Play 上一度无法购买 Pro。两种说法来自不同独立来源，你实际看到的以商店页面与应用内显示为准（截至 2026-09 检索时的状态，随时会变）。
2. **服务器侧的 Premiere 是独立的一件事**：Live TV、跳过片头、硬件加速转码这些在 Emby 官方口径里属于服务器 Premiere 功能，换哪个客户端都绕不开 —— 详见第三节。

## 三、功能差异对照

| 能力 | 官方 Emby for Android | Yamby |
| --- | --- | --- |
| 投屏 | 支持 Chromecast | 公开资料未提投屏 |
| 弹幕（dandanplay 中文弹幕） | 公开资料未提 | 有 |
| Strm 直通播放 | 走通用播放链路 | 明确支持 direct play |
| 播放器手势 | 按官方客户端自己的交互设计 | 可开关的手势控制 |
| 主题与外观 | 客户端主题依赖服务器 Premiere | 10 多种主题（Pro） |
| MPV 内核 | 无 | Pro 可启用 |
| 离线下载 | 需 Premiere | 公开资料未提下载功能 |
| 平台 | 手机、平板、Android TV/Fire TV（独立 app） | 手机、平板 |

**两边共同受服务器限制的**：Live TV 需要服务器有 Premiere；跳过片头需要服务器支持（官方口径里也是 Premiere 功能）；硬件加速转码需要 Premiere（Nvidia Shield 与 WD NAS 设备除外）。服务器没开这些，客户端再强也出不来 —— 换客户端前先确认瓶颈在不在服务器。

## 四、社区里的实际反馈

- 有 V2EX 用户反馈 Yamby 的界面加载与操作响应比同类客户端快（2024 年发帖时的体验，个例，供参考）。
- 有第三方客户端对比指南（2026-09 核验）把 Yamby 归为「大部分功能当前可免费使用」，与前文开发者渠道的说法互相印证。
- 多名用户在开发者发布帖下问过 TV 版，截至目前官方描述仍只写手机与平板。

## 五、怎么选

- 只在手机/平板上看、想要弹幕与 Strm 直通、不想为播放解锁付费 → Yamby 这条线；
- 需要 TV 端、需要 Chromecast 投屏、或所在服务端白名单只认官方客户端 → 官方 Emby for Android（TV 用官方 TV 版）；
- 拿不准就两个都装：连的是同一台服务器，账号通用，来回切一次就知道差别在哪（连接方法见 [连接Emby服务器教程.md](连接Emby服务器教程.md)）。

如果你的服务器是 Jellyfin 而不是 Emby，先看 [连接Emby服务器教程.md](连接Emby服务器教程.md) 第一节关于兼容口径的说明再决定。
