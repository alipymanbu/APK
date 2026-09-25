# Learn Kali Linux 和真正的 Kali 系统有什么区别

> 它是教程 App 还是 Kali 系统本体？想在手机上真跑 Kali 该走哪条路。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [教程内容与学习路线.md](教程内容与学习路线.md) · [虚拟机练习环境搭建教程.md](虚拟机练习环境搭建教程.md) · [常见问题与解决方法.md](常见问题与解决方法.md)

---

> [!IMPORTANT]
> **Learn Kali Linux 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/98a282504e4c](https://pan.quark.cn/s/98a282504e4c)

---

## 一、一句话结论

Learn Kali Linux 是一个**教学应用**——它装的是关于 Kali Linux 的图文教程，18M 出头的体积里没有 Kali 系统镜像；你装上它 ≠ 手机上有了 Kali。真正的 Kali Linux 是电脑上跑的发行版，想在安卓手机上真跑 Kali，官方路线是 Kali NetHunter。三者对照如下：

| | Learn Kali Linux（本 App） | Kali Linux（系统） | Kali NetHunter（手机上的 Kali） |
| --- | --- | --- | --- |
| 是什么 | 安卓上的图文教程 | Debian 系渗透测试发行版 | 官方的安卓移动平台 |
| 形态 | 一个 18.10M 的应用 | 系统镜像（装在电脑/虚拟机） | 系统级容器 + 应用 |
| 能不能执行 Kali 命令 | 不能，只教你命令是什么 | 能 | 能 |
| 典型用法 | 手机上读教程、认工具 | 电脑虚拟机里练习 | 手机上跑 Kali 工具 |
| Root 要求 | 不需要 | 不涉及 | Rootless 版不需要 |

## 二、本 App 到底给你什么

- **给你的是知识**：分章图文，讲 Kali 基础、常用命令、工具用途，界面英文（见 [教程内容与学习路线.md](教程内容与学习路线.md)）；
- **不给你的是环境**：没有终端可敲、没有工具可运行、没有系统可启动——章里的配图是电脑 Kali 桌面的截图，是「给你看」不是「给你用」；
- 它的定位类似一本口袋教材。教材里讲再多元器件，也不等于你手里有了实验台。

## 三、真想在手机上跑 Kali：官方路线是 NetHunter

Kali 官方（kali.org）提供的安卓方案叫 **Kali NetHunter**，分三个版本，按你手机改不改系统选：

| 版本 | 需要 Root | 说明 |
| --- | --- | --- |
| NetHunter Rootless | 不需要 | 原生未改的手机即可装，基于 Termux，含 Kali 命令行与桌面（KeX） |
| NetHunter Lite | 需要 Root | 完整包，无定制内核 |
| NetHunter | 需要 Root + 定制内核 | 全功能，Wi-Fi 注入、HID 等硬件级能力只在这版 |

- 官方入口：[www.kali.org](https://www.kali.org) → Get Kali → Mobile，以及文档页 [www.kali.org/docs/nethunter](https://www.kali.org/docs/nethunter)（版本能力表、Rootless 安装步骤都在上面，具体以官方页面当时显示为准）；
- Rootless 版对新手最友好：不改系统、不失去保修，代价是部分依赖内核的功能用不了；
- Rootless 的安装入口是官方 Rootless 文档（NetHunter Store 装 Termux → 跑官方安装脚本），装完常用命令如 `nethunter`（可缩写 `nh`）进入环境、`nethunter kex &` 开桌面会话——命令表以官方页面当时显示为准；
- 官方文档自己也列了几条使用限制，动手前先知道：Metasploit 在 Rootless 里没有数据库支持；`top` 这类需要内核访问的工具在未 Root 手机上跑不起来；部分三星机型会拦 `sudo`，官方建议改用 `su -c`；
- NetHunter 是**系统级安装**，比装一个教程 App 复杂得多（镜像几个 GB、要预留十几 GB 存储），动手前先把官方文档通读一遍。装的过程中报错怎么排，通用思路与 [命令报错与排查速查.md](命令报错与排查速查.md) 一致（层次：权限 / 网络 / 源）。

## 四、怎么选

- 只是想**认识** Kali、准备入门 → 装本教程 App 就够，从 Let's Start 开始读；
- 想**动手敲命令**、在受控环境练习 → 电脑虚拟机装 Kali，步骤见 [虚拟机练习环境搭建教程.md](虚拟机练习环境搭建教程.md)，教程配图与你的屏幕能对上；
- 想**手机上真跑** Kali 工具 → 走官方 NetHunter Rootless，别指望本 App 提供这个能力。

无论走哪条路，练习只在自己的设备与自建环境里做；对非自有系统与网络使用安全工具是违规的，这一点对三条路线同样适用。
