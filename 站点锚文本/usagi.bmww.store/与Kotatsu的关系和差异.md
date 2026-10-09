# Usagi 和 Kotatsu 什么关系：出身、谱系与差异

> 搜这个问题的人，多半是发现两者界面长得像，或者听说前者是后者的分支。这篇把血统、协议、改动点一次说清。
> **相关文档**：[图源插件导入与管理.md](图源插件导入与管理.md) · [订阅追更与离线阅读.md](订阅追更与离线阅读.md)

---

## 一、它从哪里来

Usagi 是一款安卓漫画阅读器，开源协议是 GPL-3.0，源码挂在 GitHub 的 UsagiApp/Usagi 仓库下（见 [github.com/UsagiApp/Usagi](https://github.com/UsagiApp/Usagi)）。项目仓库的初始提交注明代码来自 Yumemi 项目，而 Yumemi 本身出自 Kotatsu 一脉——所以你在 Usagi 里看到的四栏布局、图源概念、Material 风格设置页，都能在 Kotatsu 系应用里找到影子。

它也在 F-Droid 应用商店上架（包名 `org.draken.usagi`，见 [f-droid.org/en/packages/org.draken.usagi](https://f-droid.org/en/packages/org.draken.usagi/)），国际化的翻译工作放在 Weblate 上进行，中文界面就是社区翻译的成果。

## 二、它自己改了什么

分支不是照搬。从项目说明和更新记录看，Usagi 在原版基础上做了这些调整：

- **修补旧问题**：把原版长期被反馈的功能缺陷逐个修掉，稳定性优先；
- **低端机优化**：对低配手机做专项优化，系统支持下探到 Android 5.0——旧机器不用淘汰；
- **JAR 图源插件**：图源改成以单个 JAR 文件导入的形式，装什么自己定（操作见[图源插件导入与管理.md](图源插件导入与管理.md)）；
- **界面风格**：新增 Modern、Classic 等多套界面风格，配色、背景、视图选项可调；
- **配套服务**：同步服务、代理等周边组件保持定期更新的节奏。

## 三、同源应用怎么选

把事实摆出来，选择你自己做：

| 维度 | Kotatsu | Usagi |
| --- | --- | --- |
| 图源方式 | 内置图源，开箱即用 | JAR 插件导入，按需加载 |
| 旧设备支持 | 常规版本线 | 专项优化，Android 5.0 起可用 |
| 界面自定义 | Material 风格 | 多套风格（Modern/Classic） |
| 协议 | 开源 | GPL-3.0 开源 |

两款都是免费开源应用，装哪个主要看你更在意开箱即用还是自主可控。想先看看 Usagi 的实际界面和安装流程，可以翻这份 [Usagi 兔子漫画阅读器介绍](https://usagi.bmww.store/)，两页说明把工作台、设置和安装步骤都截图讲了一遍。
