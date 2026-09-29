# Shizuku 的 rish 终端命令怎么用

> rish 是 Shizuku 官方自带的终端工具，让你在手机上的终端应用里直接执行以 ADB 权限运行的命令，不用为一条命令专门连电脑。本篇讲怎么导出、怎么跑第一条命令。
> **相关文档**：[连接电脑用ADB启动.md](连接电脑用ADB启动.md) · [配套应用授权与常见用途.md](配套应用授权与常见用途.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **Shizuku 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/fd8f324df974](https://pan.quark.cn/s/fd8f324df974)

---

## 一、rish 是什么、适合谁

你在电脑上用 `adb shell` 执行的命令，rish 让你在手机上的终端应用（Termux、aShell 之类）里直接敲。前提只有一个：Shizuku 处于「正在运行」状态——它停了，rish 也跟着失效。

两条路按你的终端应用选：终端应用本身支持 Shizuku（如 aShell），按 [配套应用授权与常见用途.md](配套应用授权与常见用途.md) 授权就行；终端应用本身不支持（如 Termux），才需要走下面的导出流程。

## 二、从 Shizuku 导出 rish 文件

1. 打开 Shizuku，找到「在终端应用中使用 Shizuku」的教程入口；
2. 按应用内教程导出 rish 相关文件（默认导出到 `sdcard/Shizuku` 文件夹）；
3. 应用内教程会要求把 rish 脚本里的应用包名改成你终端应用的包名（脚本里有 `RISH_APPLICATION_ID` 一行），照教程改完保存；
4. 在终端应用里切到脚本所在目录，执行 `sh rish`，进入 Shizuku shell。

## 三、安卓 14 及以上的存放位置限制

官方 v13.5.2 的更新说明明确：安卓 14 起系统不允许把 rish 放在 `/sdcard` 直接运行，需要把文件复制到终端应用自己的数据目录再用。导出后报权限错误、跑不起来，先检查是不是这一条。

## 四、基本用法

进入 rish 后，敲的命令就以 ADB 权限执行。也可以不进交互界面，单条执行：

```bash
rish -c 'ls'
```

想换用系统里其他 shell 执行：

```bash
rish exec /路径/其他shell
```

在 Termux 里遇到 `cd` 之类命令行为异常时，官方 rish 提供 `RISH_PRESERVE_ENV` 环境变量控制环境变量的处理方式，取值说明见 [rish 官方文档（GitHub）](https://github.com/RikkaApps/Shizuku-API/tree/master/rish)。

执行报错时先确认 Shizuku 还在运行——服务停掉后的报错和「命令本身写错了」是两回事，服务问题先看 [常见问题与故障排查.md](常见问题与故障排查.md)。
