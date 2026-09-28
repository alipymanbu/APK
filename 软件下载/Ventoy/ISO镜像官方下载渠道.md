# ISO镜像官方下载渠道

> 做启动盘的前提是手里有系统镜像。本篇列 Windows 与常见 Linux 发行版的官方下载入口，以及下载时的三个注意点。
> **相关文档**：[手机制作启动U盘完整步骤.md](手机制作启动U盘完整步骤.md) · [启动U盘在电脑上怎么用.md](启动U盘在电脑上怎么用.md)

---

> [!IMPORTANT]
> **Ventoy 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/3dc90dc22c5c](https://pan.quark.cn/s/3dc90dc22c5c)

---

## 一、Windows

| 系统 | 官方入口 |
| --- | --- |
| Windows 11 | [微软官方下载页](https://www.microsoft.com/software-download/windows11) |
| Windows 10 | [微软官方下载页](https://www.microsoft.com/software-download/windows10) |

- 两个页面里都有「下载磁盘映像（ISO）」的选项，选它、选语言、确认后就会给出几十小时的直链下载地址（链接有时效，过期重新选一遍即可）；
- 页面内容会随版本更新变化，当前是哪个版本、给哪个架构，以页面当时显示为准；
- Windows 官方页有一处要注意：Windows 10 从 2025 年 10 月 14 日起不再有免费安全更新，仍在用它的机器要有这个预期；
- 手机浏览器同样能打开这两个页面直接下载——正好配合用手机做启动盘的流程，但要留意流量：单个镜像普遍 5GB 上下。

## 二、Linux 发行版

| 发行版 | 官方入口 |
| --- | --- |
| Ubuntu | [Ubuntu 官方下载](https://cn.ubuntu.com/download) |
| Debian | [Debian 官方下载](https://www.debian.org/download) |
| Fedora | [Fedora 官网](https://fedoraproject.org) |
| Arch Linux | [Arch Linux 官方下载](https://archlinux.org/download) |
| openSUSE | [openSUSE 官网](https://www.opensuse.org) |

版本号和页面会随时间变化，以各官网当时显示为准。Ubuntu 这类发行版还提供国内可直连的官方镜像站点，下载速度比主站快时优先用镜像站。

## 三、下载时的三个注意点

1. **认准官方渠道**：系统镜像决定了整台电脑跑的是什么，来路不明的「优化版」「精简版」镜像不要用，官方页面的才可靠；
2. **单个文件普遍超过 4GB**：Windows 10/11 的 64 位镜像通常 5GB 上下，FAT32 文件系统的分区放不下，做盘时给U盘选 exFAT 或 NTFS，做法见 [手机制作启动U盘完整步骤.md](手机制作启动U盘完整步骤.md) 的分区方案一节；
3. **镜像不完整会白做一趟盘**：下载中断、传输截断的镜像往往能拷进U盘、却启动到一半报错；对完好的判断是——部分官方页面提供校验值，能核对就核对，对不上或没法核对时重新下载一份是最省事的处理。
