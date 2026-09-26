# 用 Obtainium 自动更新 Chronos

> Chronos 不在应用商店上架，本篇讲用 Obtainium 盯住官方发布页、有新版就提醒你（乃至后台自动装）。
> **相关文档**：[版本更新注意事项.md](版本更新注意事项.md) · [下载与安装教程.md](下载与安装教程.md) · [闹钟不响怎么办.md](闹钟不响怎么办.md)

---

> [!IMPORTANT]
> **Chronos 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/21ab268dc0fc](https://pan.quark.cn/s/21ab268dc0fc)

---

## 一、为什么侧载应用需要这么个东西

Chronos 走的是 GitHub Releases 分发，没有 Google Play 就没有商店级的自动更新 —— 不管它，就只能自己每隔段时间去 [GitHub Releases](https://github.com/meenbeese/Chronos/releases) 看有没有新版。Obtainium 是一个开源的「侧载应用更新管理器」：你把应用的发布页地址一次性交给它，它在后台盯着这个页面，有新版就通知你、符合条件时直接帮你装。官方 README 也提供了 Obtainium 深链，说明开发者本人认可这条路。

## 二、装 Obtainium 并添加 Chronos

1. 到 [obtainium.imranr.dev](https://obtainium.imranr.dev/) 下载 Universal APK 并安装（装它本身也要走一次「允许未知来源」，和 [下载与安装教程.md](下载与安装教程.md) 第三节一样）。
2. 打开 Obtainium，切到「Add app（添加应用）」页。
3. 把 `https://github.com/meenbeese/Chronos/releases` 粘贴进「App source URL」，点添加 —— 它会自动识别为 GitHub 源、抓到包名 `com.meenbeese.chronos`。
4. 确认识别出的包名与已装的 Chronos 一致，保存。之后有新版本，Obtainium 会发通知，点进去就是「安装」按钮，覆盖安装即可（更新前的备份事项见 [版本更新注意事项.md](版本更新注意事项.md) 第二节）。

也可以在浏览器里打开 Chronos 的 GitHub Releases 页、复制地址栏 URL，再切到 Obtainium 粘贴 —— 效果一样。

## 三、能不能后台自动装（静默更新）

按 Obtainium 官方 Wiki（截至其文档所示），后台静默安装更新要同时满足四个条件：

1. 系统是 **Android 12 或更高**；
2. 被更新的应用**目标 API 级别较新**（Chronos 2025.3.1 起 target 16，满足）；
3. 当前装的这个版本**当初就是由 Obtainium 装的**（手动装的 APK 不满足 —— 想走全自动，先在 Obtainium 里重装一次 Chronos）；
4. Obtainium 的后台更新开关处于打开（默认开）。

四条不全满足也不影响核心功能：Obtainium 照样会在有新版时通知你，只是安装那一步要你手动点。

## 四、用着用着不提醒了

按这个顺序查：

1. Obtainium 里 Chronos 还在跟踪列表里吗（被误删就重新按第二节添加）；
2. 系统有没有把 Obtainium 的后台与通知权限收掉 —— 它被杀后台就发不出提醒，处理办法与 [闹钟不响怎么办.md](闹钟不响怎么办.md) 第二节同理（把 Obtainium 也加入电池优化白名单）；
3. GitHub 源的请求频率受平台限制，Obtainium 对 GitHub 源有请求配额说明（见其 Wiki 的 GitHub 一节）—— 长期不提醒时打开应用手动点一次「检查更新」验证。

## 五、不用 Obtainium 的替代路线

- **IzzyOnDroid 仓库**：用 Neo Store、Droid-ify 这类支持第三方 F-Droid 仓库的客户端添加 [IzzyOnDroid 仓库](https://apt.izzysoft.de/fdroid/index/apk/com.meenbeese.chronos)，更新走商店式提示（详见 [版本更新注意事项.md](版本更新注意事项.md) 第三节）；
- **纯手动**：隔段时间去 [GitHub Releases](https://github.com/meenbeese/Chronos/releases) 对一眼版本号（怎么判断新旧见 [版本更新注意事项.md](版本更新注意事项.md) 第一节），下载 APK 覆盖安装。

哪条路都行，差别只在「谁替你记着版本这件事」。
