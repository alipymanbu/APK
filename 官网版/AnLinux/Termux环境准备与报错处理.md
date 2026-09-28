# AnLinux 的 Termux 前置环境与报错处理

> AnLinux 的脚本都靠 Termux 执行；Termux 本身没准备好，后面步步是坑。本篇讲渠道选择、首次初始化，以及 Android 12 以上的杀进程问题。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [安装Linux系统教程.md](安装Linux系统教程.md) · [常见问题与解决方法.md](常见问题与解决方法.md)

---

> [!IMPORTANT]
> **AnLinux 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e53d7558362d](https://pan.quark.cn/s/e53d7558362d)

---

## 一、Termux 从哪装：渠道差异比你想的大

Termux 官方已把 Google Play 渠道标注为弃用（Deprecated），Play 商店里那份多年没有更新，装它会在后面的脚本执行里遇到各种莫名报错。建议按这个优先级选：

1. **F-Droid**：[https://f-droid.org/packages/com.termux/](https://f-droid.org/packages/com.termux/)
2. **GitHub 发布页**：[https://github.com/termux/termux-app/releases](https://github.com/termux/termux-app/releases)

两条铁律：

- **F-Droid 版与 Play 版签名不同，不能互相覆盖升级**，只能卸载一个再装另一个；卸载 Play 版会连数据一起清掉
- 选定一个渠道后一直用它更新，别来回换

## 二、装好 Termux 先做这三件事

进 Termux 后逐条执行：

```bash
pkg update && pkg upgrade
pkg install wget proot
termux-setup-storage
```

| 命令 | 作用 |
| --- | --- |
| `pkg update && pkg upgrade` | 刷新软件源、更新自带包；跳过它，老版本的 proot 是后面报错的常见根源 |
| `pkg install wget proot` | AnLinux 脚本的直接依赖，缺了会报 `wget: command not found` 一类错误 |
| `termux-setup-storage` | 申请存储访问权限，弹窗允许即可；后面想和手机相册、下载目录互传文件靠它 |

都跑完再回 AnLinux 复制安装命令。

## 三、Android 12 及以上：signal 9 杀进程问题

**现象**：Termux 跑着跑着突然跳出 `[Process completed (signal 9) - press Enter]`，安装到一半的脚本直接断掉。这不是 Termux 或 AnLinux 的 bug —— Android 12 起系统会清理应用的「幽灵后台进程」：全系统子进程超过 32 个，或某个进程后台占用 CPU 过高，直接 SIGKILL（详见 Termux 官方仓库的 [issue #2366](https://github.com/termux/termux-app/issues/2366)）。

按系统版本处理（以 Termux 官方文档为准）：

| 系统版本 | 处理办法 |
| --- | --- |
| Android 14 及以上 | 开发者选项里打开「停用子进程限制」（Disable child process restrictions），重启生效 |
| Android 12 / 12L / 13 | 原生系统可在开发者选项的 Feature flags 里关掉 phantom process 监控；国产定制系统（MIUI、ColorOS、OneUI 等）多无此开关，需电脑端 ADB 执行 `adb shell settings put global settings_enable_monitor_phantom_procs false` |
| 已 Root | ADB 命令可直接在手机上用 `su` 执行 |

即便处理过，长任务被杀的概率依然存在：系统更新可能重置开关，跑大任务时留意一下。

## 四、Termux 层面报错的快速对照

| 报错 | 原因 | 处理 |
| --- | --- | --- |
| `wget: command not found` | 没装 wget | `pkg install wget proot` |
| `proot error: '/usr/bin/env' not found` | Termux 或 proot 版本过旧 | `pkg update && pkg upgrade` 后重新执行安装命令 |
| `repository is under maintenance` | 软件源临时不可用 | 过段时间重试，或换源（见 Termux 官方 wiki） |
| `[Process completed (signal 9)]` | 系统杀后台进程 | 见上一节 |

处理完这些，再回到[安装Linux系统教程.md](安装Linux系统教程.md)继续装发行版；安装包还没就绪的先看[下载与安装教程.md](下载与安装教程.md)。
