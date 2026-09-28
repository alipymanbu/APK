# Obtainium 的 F-Droid 版与官方版有什么区别

> 网上能下到两个「Obtainium」：GitHub 官方发布版和 F-Droid 版。两者包名不同、可以并存。本篇讲差在哪、怎么确认自己装的是哪个、更新各走哪条路。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [支持的应用源有哪些.md](支持的应用源有哪些.md) · [更新失败与常见问题排查.md](更新失败与常见问题排查.md)

---

> [!IMPORTANT]
> **Obtainium 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/626e3e3f7b27](https://pan.quark.cn/s/626e3e3f7b27)

---

## 一、两个版本各是什么

| | 官方版（GitHub 发布版） | F-Droid 版 |
| --- | --- | --- |
| 包名 | dev.imranr.obtainium | dev.imranr.obtainium.fdroid |
| 分发渠道 | GitHub Releases、官网直接下载 | F-Droid 官方仓库、IzzyOnDroid 等 |
| 功能与界面 | 一致 | 一致 |

差别只在包名与分发渠道 —— 功能、设置项、支持的应用源没有任何不同。F-Droid 版在包名后加了 `.fdroid` 后缀，是应用商店生态里区分同一应用不同渠道安装包的通行做法。

## 二、能不能同时装、数据通不通

包名不同，系统当成两个应用，可以并存（两版图标一样，建议只留一个，或给其中一个在桌面改名区分）。两个版本的应用列表互不相通 —— 一个里添加的应用不会出现在另一个里；要搬列表就用应用内的导入导出功能，导成 json 文件再在另一边导入。

## 三、怎么确认手机上装的是哪个

系统设置 → 应用管理 → 找到 Obtainium → 应用详情里看包名：带 `.fdroid` 后缀的就是 F-Droid 版。Obtainium 自己的设置页底部也留有查看入口。

本文档对应的[Obtainium 安装文件资源（夸克网盘）](https://pan.quark.cn/s/626e3e3f7b27)就是 F-Droid 版：包名 `dev.imranr.obtainium.fdroid`、版本 1.6.10。

## 四、更新各走哪条路

- **F-Droid 版**：跟着 F-Droid 渠道走 —— 用 F-Droid 客户端管理，或在 Obtainium 里把它自己的 F-Droid 页面（`https://f-droid.org/packages/dev.imranr.obtainium.fdroid`）添加进去，让它自我跟踪更新；
- **官方版**：把它的 GitHub 发布页（`https://github.com/ImranR98/Obtainium/releases`）添加进 Obtainium，新版一出就会提醒或按条件自动装。

两个版本都不要跨渠道更新：包名不同，拿另一个渠道的包去「升级」不会覆盖原应用，只会多装一个。

## 五、对来源有要求的话，怎么验证

官方 Wiki 给出了签名证书的 SHA-256 指纹，两个版本共用同一套签名（F-Droid 版是可复现构建，与同一源码的构建结果逐字节一致）：

`B3:53:60:1F:6A:1D:5F:D6:60:3A:E2:F5:0B:E8:0C:F3:01:36:7B:86:B6:AB:8B:1F:66:24:3D:A9:6C:D5:73:62`

需要核对安装包时，用签名校验工具比对这串指纹；这份网盘包的 MD5 见[下载与安装教程.md](下载与安装教程.md)的安装包信息表。
