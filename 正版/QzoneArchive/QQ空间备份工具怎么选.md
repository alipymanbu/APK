# QQ空间备份工具怎么选

> 本篇对比三款常见的 QQ 空间备份工具，按你的设备和需求对号入座；三款内容各有侧重，也可以组合使用。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [归档范围与内容限制.md](归档范围与内容限制.md) · [HTML导出与离线浏览方法.md](HTML导出与离线浏览方法.md)

---

> [!IMPORTANT]
> **QzoneArchive 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/da7ac7d0f816](https://pan.quark.cn/s/da7ac7d0f816)

---

## 一、先看结论

| 你的情况 | 更合适的选择 |
| --- | --- |
| 只想在手机上操作，装个应用就跑 | QzoneArchive（安卓版安装见 [下载与安装教程.md](下载与安装教程.md)） |
| 想把日志、私密日记、独立相册、收藏夹、好友列表一起备份 | QZoneExport（浏览器扩展，在电脑上用） |
| 只想要历史说说的文字记录，越省事越好 | GetQzonehistory（电脑上运行的开源脚本） |
| 想备份得尽量全 | 三款内容各有侧重，可以组合使用 |

## 二、三款工具的对比

以下是截至 2026 年 9 月的信息，以各自官方页面为准：

| 对比项 | QzoneArchive | QZoneExport | GetQzonehistory |
| --- | --- | --- | --- |
| 形态 | 独立应用，有安卓版 | 浏览器扩展（电脑） | Python 脚本（电脑运行） |
| 平台 | Windows / macOS / Linux / Android | Chrome、Firefox 等主流浏览器 | Windows / macOS / Linux（需 Python 环境） |
| 能备份什么 | 动态、照片、视频、评论、点赞、留言板，按互动关系整理 | 说说、日志、私密日记、相册、视频、留言板、好友、收藏夹、分享、访客 | 历史说说（导出 Excel） |
| 特色 | 断点续传、媒体时光轴、互动排行榜 | 覆盖的内容类型最全 | 能找回部分已删的说说 |
| 使用门槛 | 安装即可用 | 要在电脑浏览器里装扩展 | 要会装 Python 依赖 |
| 维护状态 | 2026 年 9 月 4 日起停止维护 | 持续更新中 | 以其官方仓库为准 |

QZoneExport 的官方仓库：[gitee.com/mirrors_ShunCai/QZoneExport](https://gitee.com/mirrors_ShunCai/QZoneExport)；GetQzonehistory 的官方仓库：[github.com/LibraHp/GetQzonehistory](https://github.com/LibraHp/GetQzonehistory)。无论用哪款，都只用官方仓库发布的版本——冒名的"新版""修复版"一直存在。

## 三、三款工具各自的边界

- **QzoneArchive**：靠互动记录抓内容，没人互动过的动态可能拿不到（详见 [归档范围与内容限制.md](归档范围与内容限制.md)）；不覆盖日志与独立相册。
- **QZoneExport**：内容类型覆盖最全，但整个流程要在电脑浏览器里完成，只想用手机的人不合适。
- **GetQzonehistory**：走历史消息列表，仅自己可见的说说拿不到（项目 README 原话）；输出是 Excel 表格，翻照片和视频不如前两款直观。

## 四、可以组合，互不冲突

三款工具各管一块，都只登录你自己的账号，先后顺序不影响结果：

1. 手机上用 QzoneArchive 把动态、照片、留言归档并导出 HTML（方法见 [HTML导出与离线浏览方法.md](HTML导出与离线浏览方法.md)）；
2. 电脑上用 QZoneExport 补日志、独立相册、收藏夹这些它不覆盖的类型；
3. 所有导出文件都多存一份，通用原则见 [数据存储与账号安全.md](数据存储与账号安全.md)。
