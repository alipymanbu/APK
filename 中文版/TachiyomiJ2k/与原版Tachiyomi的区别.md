# TachiyomiJ2K 与原版 Tachiyomi 的区别

> 搜「Tachiyomi」时你大概率会同时看到原版、Mihon 和一堆衍生版。本篇用可核实的事实理清 TachiyomiJ2K 在这个家族里的位置：它改了什么、和谁同源、怎么按自己的情况挑。
> **相关文档**：[界面与阅读设置.md](界面与阅读设置.md) · [下载与安装教程.md](下载与安装教程.md) · [备份与数据迁移.md](备份与数据迁移.md)

---

> [!IMPORTANT]
> **TachiyomiJ2K 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/c26c5183473b](https://pan.quark.cn/s/c26c5183473b)

---

## 一、先交代背景

- 原版 Tachiyomi 在 2024 年 1 月宣布停止开发。
- TachiyomiJ2K 是基于原版代码的衍生版（fork），由 Jays2Kings 维护，Apache 2.0 开源，代码在 [github.com/Jays2Kings/tachiyomiJ2K](https://github.com/Jays2Kings/tachiyomiJ2K)。
- 原版的代码由社区以 [Mihon](https://mihon.app/) 的名义继续开发；TachiyomiJ2K 的官方说明也写明「based on the original Tachiyomi, now continued as Mihon」。
- 这几个应用共用同一套扩展机制和备份格式（`.tachibk`），书架可以在它们之间直接搬家。
- 原版停更时，官方扩展仓库也一并下架——不管用哪家的衍生版，都要手动添加社区扩展仓库才能装源，见 [扩展列表空白与源失效排查.md](扩展列表空白与源失效排查.md)。

## 二、TachiyomiJ2K 在原版之上加了什么

原版的功能（多来源在线阅读、本地下载、分类书架、追踪同步、定时更新、备份）它全都有，在此之上衍生版自己加的部分：

| 类别 | 内容 |
| --- | --- |
| 界面 | 详情页按封面配色主题化；浮动搜索栏；加宽的单手工具栏（可调回）；书架单列表视图与错落网格；拖拽排序 |
| 阅读 | 阅读中直接看全部章节；双页合并（针对平板） |
| 组织 | 动态分类（按标签 / 追踪状态 / 来源自动分组）；Recents 页（新增、更新、接着读集中一页）；统计页 |
| 效率 | 批量换源；动态快捷方式（桌面长按直达上次的章节）；删除带撤销 |
| 系统 | Android 10 分享面板升级；Android 12 的应用与扩展自动更新 |

## 三、和 Mihon 的关系与差别

Mihon 是原版的直系续作，TachiyomiJ2K 是另一条线上的衍生版，两者都在更新（截至 2026-09，TachiyomiJ2K 的 [Releases 页面](https://github.com/Jays2Kings/tachiyomiJ2K/releases) 上仍在发布新版本）。几条可核实的事实差异：

| 项目 | TachiyomiJ2K | Mihon |
| --- | --- | --- |
| 系统要求 | Android 6.0 起 | 官方标注 Android 8.0 起 |
| 导航形态 | 经典侧边栏抽屉 | 底部导航栏（Material 3） |
| 界面风格 | 衍生版自己重做的一套，可调项偏多 | 跟随原版延续，形态更收敛 |

## 四、怎么按自己的情况挑

- 手机系统是 Android 6.0~7.x → 在这个家族里，TachiyomiJ2K 的系统门槛更低，Mihon 装不上。
- 习惯原版那种侧边栏、想把界面和阅读器的细节逐项调一遍 → 衍生版暴露的开关更多。
- 用平板看书多 → 双页合并就是为这个场景做的。
- 只想要个装完就能用、导航形态跟主流应用一致 → Mihon 的底部导航更接近这个习惯。

无论选哪个，书架与进度都靠同一套备份格式互通（见 [备份与数据迁移.md](备份与数据迁移.md)），先装一个试、不合意再搬家，代价只是重装一遍扩展。
