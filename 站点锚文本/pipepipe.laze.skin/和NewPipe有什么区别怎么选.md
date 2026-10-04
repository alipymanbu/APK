# 和 NewPipe 有什么区别怎么选

> 两个应用名字里都有 Pipe，界面也长得像，但它们已经是各自独立开发的两个项目。这篇讲清渊源、差异与选择思路。

---

## 同一棵树长出的两棵树

PipePipe 在 2022 年初从 NewPipe 分叉，之后与上游「双向隔离」：不接收 NewPipe 的更新，也不向上游回推改动。分叉时间早、改动大，两者连包名都不同——NewPipe 是 `org.schabi.newpipe`，PipePipe 是 `InfinityLoop1309.NewPipeEnhanced`，装在同一台手机上互不冲突。

对使用者来说，这个渊源的实际含义是：界面操作习惯接近，会刷 NewPipe 的人上手 PipePipe 几乎没有学习成本，搬家也容易——NewPipe 的备份数据可以直接导入 PipePipe，应用内自动处理格式。

## 差异主要在四块

- **平台**：NewPipe 覆盖 YouTube、PeerTube、SoundCloud、Bandcamp 等，平台面更广；PipePipe 主攻 YouTube、BiliBili、NicoNico 三家，B 站与 NicoNico 是原版没有的。
- **跳过赞助片段**：PipePipe 集成了 SponsorBlock，YouTube 与 B 站都能用；NewPipe 没有。
- **登录**：NewPipe 不支持登录；PipePipe 可以登录 B 站或 YouTube 观看账号有权限的内容（比如大会员清晰度），Cookie 在哪些请求里携带还能在设置里配置。
- **播放与管理**：直播弹幕式聊天、睡眠定时器、音乐播放器模式、播放列表内部搜索与排序，这些都是 PipePipe 分叉后补上的能力。

## 怎么选

主要看 YouTube、还想听 SoundCloud 音频，选 NewPipe；要刷 B 站或 NicoNico、想跳赞助片段、需要用自己账号看会员内容，选 PipePipe。两个需求都有就都装上，包名不同不冲突，各试几天再留一个。

决定装 PipePipe 的话，安装步骤见[怎么安装到安卓手机](怎么安装到安卓手机.md)；功能与界面的完整介绍，[PipePipe 介绍站](https://pipepipe.laze.skin/)里有逐项整理与截图。
