# AxManager 和 Shizuku 有什么区别？

> 两个都不需要 Root、都靠 ADB 权限工作，但定位完全不同：一个自带功能让你直接操作，一个替其他应用传权限。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [无线调试启动与配对教程.md](无线调试启动与配对教程.md) · [插件与WebUI使用入门.md](插件与WebUI使用入门.md)

---

> [!IMPORTANT]
> **AxManager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e76a7ed9b163](https://pan.quark.cn/s/e76a7ed9b163)

---

## 一、一句话分清

- **Shizuku** 是一个「权限中转站」：它自己几乎没有功能，作用是把系统 API 的调用权交给其他应用——很多工具类应用写着「需要 Shizuku」，靠它实现免 Root 功能。
- **AxManager（Axeron Manager）** 是一个「自带功能的管理器」：Shell 执行器、非 Root 插件、WebUI 都在应用里直接用，不需要再装别的应用来「消费」这份权限。

## 二、对照表

| 维度 | AxManager | Shizuku |
| --- | --- | --- |
| 自己能干什么 | 应用内跑 Shell 命令、装插件、开 WebUI | 本体几乎没功能，等别的应用来调用 |
| 模块体系 | 内置非 Root 插件（包结构同 Magisk / KernelSU 模块） | 无模块体系，生态是「支持 Shizuku 的应用」 |
| 浏览器操作 | 有 WebUI | 无 |
| 项目状态 | 官方自述为 POC / 开放实验，仍在快速迭代 | 成熟项目，大量应用声明支持 |
| 开源协议 | Apache-2.0 | Apache-2.0 |

注：Shizuku 一侧以 [Shizuku 官网](https://shizuku.rikka.app/zh-hans/) 与 [GitHub 仓库](https://github.com/RikkaApps/Shizuku) 当前描述为准；AxManager 一侧以官方文档站为准，两边都可能随版本更新变化。

## 三、按你的目的选

- 想在一台没 Root 的手机上直接敲命令、试脚本、装模块：AxManager 的形态更对路，装上就能用，步骤见 [下载与安装教程.md](下载与安装教程.md)。
- 你装了某个明确写着「需要 Shizuku」的应用（权限管理、卸载器、改屏幕显示之类的工具）：那就装 Shizuku——那个应用认的是 Shizuku 的接口，装 AxManager 替不了它。
- 想要的是 Root 管理器（把模块刷进 `/system`、上 Xposed 框架）：两者都不是，那是 Magisk / KernelSU 的领域；AxManager 的插件体系刻意避开对 `/system` 的深度改动，见 [插件与WebUI使用入门.md](插件与WebUI使用入门.md) 第一节。

## 四、几个容易混淆的点

- AxManager 官方 README 的致谢里明确写了 Shizuku——它是 AxManager 的「起点与参考」（学的是 IPC 与基于 ADB 的权限处理思路）。所以两边的启动方式看起来很像：无线调试配对、电脑 ADB、Root 三条路，[无线调试启动与配对教程.md](无线调试启动与配对教程.md) 里的步骤在两边基本通用。
- 「AxManager 能替代 Shizuku 吗」：不能直接替代。声明「需要 Shizuku」的应用调用的是 Shizuku 的接口；AxManager 提供的是自己的一套环境。
- 选型时与其看别人文章的结论，不如直接上两边的 GitHub 仓库页，对比 stars、open issues 数量与最近提交日期——几分钟就有自己的判断。
