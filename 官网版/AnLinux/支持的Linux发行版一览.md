# AnLinux 支持的 Linux 发行版清单

> 截至 6.x 版本共 14 个发行版可选；怎么选、能不能多装，看这篇。
> **相关文档**：[安装Linux系统教程.md](安装Linux系统教程.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **AnLinux 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e53d7558362d](https://pan.quark.cn/s/e53d7558362d)

---

## 一、完整清单

官方目前支持的发行版（截至 6.x 版本，以[官方页面](https://github.com/EXALAB/AnLinux-App)为准）：

| 发行版 | 包管理器 | 常见用途 |
| --- | --- | --- |
| Ubuntu | apt | 通用，资料最多 |
| Debian | apt | 稳定、轻量 |
| Kali | apt | 安全工具集 |
| Kali Nethunter | apt | Kali 的移动端分支 |
| Parrot Security OS | apt | 安全测试向 |
| BackBox | apt | 安全测试向 |
| Fedora | dnf | 软件包较新 |
| CentOS | yum / dnf | 服务器环境习惯 |
| openSUSE Leap | zypper | 稳定版 |
| openSUSE Tumbleweed | zypper | 滚动更新 |
| Arch Linux | pacman | 滚动更新、可定制 |
| BlackArch | pacman | 基于 Arch 的安全工具集 |
| Alpine | apk | 体积小、启动快 |
| Void | xbps | 独立发行版，轻量 |

架构要求：armv7 / arm64 / x86 / x86_64 四种；手机基本都是 arm64。

## 二、怎么选

- **第一次用、没有特别偏好**：Ubuntu 或 Debian —— 网上教程最多，`apt` 装软件最顺
- **想要小体积**：Alpine —— 基础系统明显小于其他发行版；但它用的是 musl libc 而非 glibc，个别按 glibc 环境打包的软件可能装不上，介意就换 Debian
- **学习安全工具**：Kali（工具齐全）；注意部分依赖内核特性的工具在 PRoot 环境里跑不起来，这是所有发行版共同的限制，不是 Kali 特有
- **想滚动更新尝鲜**：Arch Linux 或 openSUSE Tumbleweed —— 包一直新，但出问题的概率也比固定版本高一点

选定后就可以按[安装Linux系统教程.md](安装Linux系统教程.md)动手装了。

## 三、多发行版并存

AnLinux 支持同时装多个发行版，各用各的目录与启动脚本，互不冲突：

- 切换：在 Termux 里运行对应的 start 脚本（`./start-ubuntu.sh`、`./start-kali.sh`……）
- 代价：每个都实打实占存储，装第二个之前先 `df -h` 看剩余空间
- 卸载其中一个不影响其他的，卸载方法见[安装Linux系统教程.md](安装Linux系统教程.md)第五节
