# aShell You 与 Termux 有什么区别

> 两者都顶着「命令行」的观感，定位却完全不同——讲清各自能干什么、你的需求该选哪个。
> **相关文档**：[常用ADB命令示例](常用ADB命令示例.md) · [Shizuku配置教程](Shizuku配置教程.md) · [下载与安装教程](下载与安装教程.md)

---

> [!IMPORTANT]
> **aShell You 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/60ba8c6c52c3](https://pan.quark.cn/s/60ba8c6c52c3)

---

## 一、一句话定位差

- **Termux**：官方定位是「终端模拟器 + Linux 环境」（见 [termux.dev](https://termux.dev/en/)），自带 APT 包管理器，能装 python、git、ssh 一整套命令行工具，等于在手机里养一个小 Linux。它跑在**普通应用权限**下。
- **aShell You**：官方 README 的自述是「run ADB, root and shell commands」——不模拟任何环境，专做**系统级命令**：借 [Shizuku](Shizuku配置教程.md) 拿到 shell 级权限后，执行 `pm`、`settings`、`wm` 这类改系统行为的命令。

## 二、权限差异决定了能做的事不同

| 你想做的事 | aShell You | Termux |
| --- | --- | --- |
| 卸载/隐藏预装应用（`pm uninstall --user 0`） | 能做 | 不行，受系统对普通应用的限制 |
| 改分辨率/密度（`wm`） | 能做 | 不行 |
| 实时看系统日志（`logcat`） | 能做 | 受限 |
| 装 python、git 等命令行工具 | 不做这类事 | 能做（APT） |
| 跑脚本、SSH 远程 | 不做这类事 | 能做 |

分野的根源：Termux 里执行 `pm`、`settings` 这类系统命令会因系统对普通应用的限制而失败，Android 10 之后限制更严；而 aShell You 的命令是借 Shizuku 以 shell 级权限执行的，不在这层限制之内。

## 三、怎么选

- 你的目标是**改系统行为**（清理预装、调显示、看日志）→ aShell You，配好 Shizuku 就能开工；
- 你的目标是**随身 Linux 命令行**（写脚本、装工具链）→ Termux；
- 两者不冲突，装哪个都不影响另一个。

具体命令怎么打，见 [常用ADB命令示例](常用ADB命令示例.md)。
