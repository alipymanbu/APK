# LightNovelReader 和 LNReader 的区别

> 搜「light novel reader」会同时跳出两款名字极像的安卓阅读器，本篇帮你分清自己装的是哪一款、别按错的教程操作。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [数据源与插件安装教程.md](数据源与插件安装教程.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **LightNovelReader 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7bce9a01b1dc](https://pan.quark.cn/s/7bce9a01b1dc)

---

## 一、两款应用，别搞混

安卓上有两款开源轻小说阅读器名字只差一个词：

- **LightNovelReader**：中文社区开发，官网 [lnr.nariko.org](https://lnr.nariko.org/)，GitHub 仓库 [dmzz-yyhyy/LightNovelReader](https://github.com/dmzz-yyhyy/LightNovelReader)——**本目录所有教程讲的都是它**；
- **LNReader**：国外开发者 Rajarshee Chatterjee 的项目，官网 [lnreader.app](https://www.lnreader.app/)，GitHub 组织 [github.com/LNReader](https://github.com/LNReader)——另一款完全不同的应用。

两者都是「安卓 + 开源 + 轻小说 + 插件化数据源」，所以搜索引擎和应用推荐里经常混着出现，但它们的界面、设置路径、插件体系完全不通——**照着 LNReader 的教程操作 LightNovelReader 会找不到对应菜单，反过来也一样**。

## 二、对照表

| | LightNovelReader | LNReader |
| --- | --- | --- |
| 包名 | `indi.dmzz_yyhyy.lightnovelreader` | `com.rajarsheechatterjee.LNReader` |
| 开发者 / 仓库 | dmzz-yyhyy（NightFish、yukonisen 等） | Rajarshee Chatterjee |
| 官网 | [lnr.nariko.org](https://lnr.nariko.org/) | [lnreader.app](https://www.lnreader.app/) |
| 开源许可 | Apache-2.0 | MIT |
| 界面语言 | 中文为主（Crowdin 多语言） | 英文为主（多语言） |
| 中文社区 | QQ 群 `867785526`、Telegram | 无官方中文群，主要在 Discord |
| 数据源插件 | 1.2.0 起的 APK 插件 + 1.2.1 插件市场 | 独立的插件仓库体系（官网列有 284 个社区插件） |
| 发布渠道 | GitHub Releases、F-Droid | GitHub Releases |

（截至 2026-09 的公开信息，各自以官方页面当时显示为准。）

## 三、怎么确认手机上装的是哪款

最可靠的办法是看**包名**，图标和名字都会撞，包名不会：

1. 手机「设置 → 应用管理」里找到该应用，看应用详情里的包名；
2. 也可以打开应用进「设置 / 关于」页看版本号——LightNovelReader 会显示形如 `1.2.1` 的版本信息，与本目录教程的截图对得上。

安装包本身也能辅助判断：[LightNovelReader 安装文件资源（夸克网盘）](https://pan.quark.cn/s/7bce9a01b1dc)里那份的包名就是 `indi.dmzz_yyhyy.lightnovelreader`（渠道明细见 [下载与安装教程.md](下载与安装教程.md) 第二节）。

## 四、已经装错了 / 按错教程操作了怎么办

两款应用数据完全独立，装错或混用表现为：按教程找不到「扩展插件」「书架」等菜单，或搜出来的界面截图和自己手机对不上。处理：

1. 先按上一节确认包名，明确自己装的是哪款；
2. 想用 LightNovelReader——卸载另一款后按 [下载与安装教程.md](下载与安装教程.md) 重装，再按 [数据源与插件安装教程.md](数据源与插件安装教程.md) 配数据源；
3. 两款可以同时安装、互不冲突（包名不同），但书架与进度不互通，别指望迁移；
4. 反馈问题时先说清是哪一款（附包名），否则 Issue 里容易答非所问（反馈渠道见 [常见问题与排查.md](常见问题与排查.md) 第五节）。
