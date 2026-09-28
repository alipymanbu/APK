# musicplay 和 RecordEchoes 是什么关系

> 一句话：musicplay、Music Player、RecordEchoes 指的是同一个开源播放器，本篇把名字、版本、渠道三层关系一次讲清，并列出完整版本线。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [歌词与播放设置.md](歌词与播放设置.md) · [闪退与常见问题排查.md](闪退与常见问题排查.md)

---

> [!IMPORTANT]
> **musicplay 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/cb1b4a43cde1](https://pan.quark.cn/s/cb1b4a43cde1)

---

在网上搜 musicplay，很容易被带到一个叫 RecordEchoes 的 GitHub 项目；搜 RecordEchoes，又会看到安装包叫 Music Player。三个名字指的是同一个应用：一个开源的酷狗第三方音乐播放器，本文对应的 0.3 版属于早期发布。

## 一、三个名字怎么对应

| 你见到的名字 | 它是什么 |
| --- | --- |
| musicplay / Music Player | 官方 Release 安装包文件名的写法（0.1 到 0.6 都是 `Music_Player-*.apk`），第三方软件站收录时多简写为 musicplay |
| KugouAndroidMusic | 项目在 GitHub 上的早期仓库名。0.3 发布当天，官方在 Issue 里给出的下载链接用的还是这个地址；仓库后来更名为 RecordEchoes，旧链接会自动跳转，不用特意区分 |
| RecordEchoes | 项目的公开名称，README 自述「记录回忆的声音」，仓库描述为「基于 Md3 设计的酷狗第三方音乐播放器」 |
| `com.ghhccghk.musicplay` | 安装包名（applicationId）。0.3 与 0.6 都用它，认包名就不会认错 |

项目由 GitHub 用户 ghhccghk 开发，源码与发布页在 [https://github.com/ghhccghk/RecordEchoes](https://github.com/ghhccghk/RecordEchoes)，采用 GPL-3.0 协议开源。

## 二、完整版本线

官方所有版本都发布在 GitHub 的 Releases 页面（[https://github.com/ghhccghk/RecordEchoes/releases](https://github.com/ghhccghk/RecordEchoes/releases)），日期与说明如下：

| 版本 | 发布日期 | 官方说明 |
| --- | --- | --- |
| 0.1 | 2025-05-27 | 首个发布 |
| 0.2 | 2025-06-13 | 无文字说明 |
| **0.3** | **2025-06-14** | 修复 userid 超过 Int 上限导致的崩溃；继续添加设置界面 |
| 0.5 | 2025-08-12 | 无文字说明 |
| 0.6 | 2025-12-25 | 大规模重构：倍速与音调调节、桌面卡拉OK歌词改进、睡眠定时器、VIP 信息展示、退出登录、歌词分享等 |

官方 Release 的安装包文件名从 0.1 到 0.6 都叫 `Music_Player-*`，第三方软件站收录时则多用 musicplay 这个写法；0.3 在多个软件站有收录，是流传较广的一个版本。0.6 的新功能清单见[歌词与播放设置](歌词与播放设置.md)。

两点补充：

- **官方没有发布过 0.4**，版本号从 0.3 直接跳到 0.5，中间差的是开发进度而不是你漏下了什么。
- **0.3 本身就是为修复登录问题而发的版本**：0.2 及更早的版本扫码登录会闪退（原因是账号的 userid 数值超过了程序按整数处理的上限），官方当天在 [Issue #1](https://github.com/ghhccghk/RecordEchoes/issues/1) 里全程排查并发出 0.3 修复。所以如果你在网上看到「musicplay 扫码闪退」的老讨论，答案基本都是：升到 0.3。

## 三、用 0.3 还是 0.6

- **0.3（网盘里这份，48.97M 单架构包）**：功能简单直接，装上就能用，适合只想听歌、不想折腾的用户。想直接获取这份 0.3，用本页顶部的 musicplay 安装文件资源（夸克网盘）即可。
- **0.6（2025-12-25）**：功能扩充明显（倍速、音调、睡眠定时器、桌面歌词改进等），但官方在发布说明里写明「包含大量重构内容，如遇问题建议清理数据或重新安装」。对新功能有需求再去 [Releases 页面](https://github.com/ghhccghk/RecordEchoes/releases)下载，覆盖安装即可升级。

两个版本覆盖安装不冲突（包名一致）；从 0.3 直接升 0.6 后若表现异常，按官方建议清理应用数据或卸载重装。截至本文更新，0.6 仍是官方最新发布，之后有没有新版本以 Releases 页面为准。

## 四、它和酷狗官方的关系

RecordEchoes 是第三方客户端，不是酷狗官方出品：它用你自己的酷狗账号登录，通过公开接口访问曲库、歌单与歌词。账号能听什么歌，取决于账号自身的权益，客户端不会额外改变这一点（0.6 起会在应用内显示账号的 VIP 信息）。也因为它依赖上游接口，接口变动时个别功能可能暂时失效，这类情况以项目仓库的发布与反馈为准。
