# ADB授权与高级功能设置

> 不 root 也能解锁高级功能：受限权限、`WRITE_SECURE_SETTINGS`、Shizuku 与隐藏导航栏。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题排查.md](常见问题排查.md) · [版本更新与开源信息.md](版本更新与开源信息.md)

---

> [!IMPORTANT]
> **Smart Dock 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/bfe072753368](https://pan.quark.cn/s/bfe072753368)

---

## 一、先判断你要走到哪一步

Smart Dock 装成普通应用就能用，下面这些是**可选的增强项**，按需求取用：

| 你的状况 | 需要做哪节 |
| --- | --- |
| 设置里找不到「无障碍」或「通知」开关 | 二、允许受限权限 |
| 想用全部高级设置（窗口行为、系统级调整） | 三、授予安全设置权限 |
| 不方便连电脑，但想授予权限 | 四、用 Shizuku 代替 ADB |
| 想彻底隐藏底部导航条 | 五、隐藏导航栏 |

官方还提到：把 Smart Dock 装成**系统应用**能直接拿到所需权限，但那需要改系统分区，普通用户不建议，走下面的 ADB / Shizuku 路线即可。

## 二、允许受限权限

部分机型上，应用详情页里不显示「无障碍」「通知」的授权入口。官方给的解法：

1. 系统设置 → 应用 → 找到 Smart Dock；
2. 点右上角**三个点的菜单**（不是应用自己的菜单）；
3. 选择「允许受限权限」（Allow restricted permissions）；
4. 回到应用的权限页，此时开关会出现，逐项打开。

## 三、授予安全设置权限（ADB）

很多高级功能依赖 `WRITE_SECURE_SETTINGS`（写入系统安全设置）权限，这个权限不会弹窗申请，只能用命令授。步骤：

1. 手机打开「开发者选项」（关于手机里连点版本号 7 次），打开 **USB 调试**；
2. 数据线连电脑，电脑装好 adb（命令行输入 `adb version` 能出版本号即已装好）；
3. 执行：

```bash
adb shell pm grant cu.axel.smartdock android.permission.WRITE_SECURE_SETTINGS
```

4. 命令无输出即成功，重新打开 Smart Dock，高级设置项会解锁。

不想用电脑的话，root 设备上可以在终端里以 root 身份执行同一条 `pm grant` 命令；或者用手机端的 shell 类应用执行（配合第五节的 Shizuku）。

**多用户/工作资料的情况**：如果命令提示找不到用户，先 `adb shell pm list users` 查用户 ID，再给命令加 `--user <ID>` 重跑。

## 四、用 Shizuku 免电脑授权

[Shizuku](https://shizuku.rikka.app) 是一个用 ADB 权限给其他应用授权的开源工具，官方 README 里明确把它列为系统安装的替代方案。基本流程：

1. 装好 Shizuku，按其引导启动服务（Android 11+ 可选「无线调试」方式，无需电脑）；
2. 在 Shizuku 里授权给 Smart Dock；
3. 回到 Smart Dock，需要 `WRITE_SECURE_SETTINGS` 的功能即可通过 Shizuku 授予。

从 1.15.0 起官方加入了 Shizuku 集成（测试版）：Dock 上显示真实运行任务、直接控制 Wi-Fi 与蓝牙、自由窗口的缩放与贴靠、关闭窗口 —— 这些是新版本才有的能力，1.14.1 上没有（版本差异见[版本更新与开源信息.md](版本更新与开源信息.md)）。

## 五、隐藏导航栏

底部导航条（返回/Home/多任务的横条或手势条）会占掉 Dock 的位置。官方仓库的 HideNav 文档给了几条路，按你的设备条件选：

**有 root**：
- 直接在 Smart Dock 的**高级设置**里有隐藏导航栏的选项；
- 或在 `/system/build.prop` 里加一行 `qemu.hw.mainkeys=1`（终端执行 `echo qemu.hw.mainkeys=1 >> /system/build.prop` 后重启）。

**没 root、系统分区可写（如 Linux 上的 Waydroid）**：
- 挂载系统分区后同样改 `build.prop`；
- Waydroid 用户在 Linux 侧执行 `waydroid prop set qemu.hw.mainkeys 1`。

**Android 11 以上、想不碰系统文件**：
- 官方文档提示可以走 LSPosed + GravityBox 路线（需已刷入 Magisk 与 LSPosed）：在 GravityBox 的「显示调整 → 扩展桌面模式」里选「隐藏导航栏」，之后从电源菜单切换；或者把 GravityBox 的导航栏高度/宽度调到 0%。

改 `build.prop` 有变砖风险的操作空间，动手前备份原文件；不想折腾就先用 Smart Dock 自带的手柄/全屏手势，多数场景够用。改完出问题的处理见[常见问题排查.md](常见问题排查.md)。
