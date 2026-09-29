# ADB 恢复屏幕参数命令详解

> 本篇是 `wm` 命令的速查：怎么查硬件原始分辨率、怎么恢复，以及电脑连不上手机时怎么排查。
> **相关文档**：[黑屏恢复与常见问题.md](黑屏恢复与常见问题.md) · [修改分辨率与DPI的完整步骤.md](修改分辨率与DPI的完整步骤.md) · [Shizuku授权与启动步骤.md](Shizuku授权与启动步骤.md)

---

> [!IMPORTANT]
> **Screener 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/8f8ba9e59be4](https://pan.quark.cn/s/8f8ba9e59be4)

---

## 一、为什么要专门讲这条命令

Screener 的修改走的就是系统 `wm` 命令同一套机制，恢复也用它——屏幕改坏了、10 秒自动撤销又没等到生效时（这种现场怎么判断见 [黑屏恢复与常见问题.md](黑屏恢复与常见问题.md) 第一节），用电脑执行两条 reset 就能把分辨率和 DPI 拉回系统默认。

先装 ADB：下载 Google 官方的 [SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools)，解压到任意文件夹，在这个文件夹里打开命令行窗口，输入 `adb` 出现帮助文本即安装成功。用 PowerShell 时命令要写成 `./adb`。

## 二、先看当前是什么参数（没抄原始值也能找回来）

```bash
adb shell wm size
adb shell wm density
```

输出形如：

```text
Physical size: 1920x1080
Override size: 1440x900
```

- `Physical size` 是硬件原始值——**当初没抄下原始参数也没关系，这条命令能直接读出来**；
- `Override size` 是被改过的覆盖值，有这一行说明当前正处于覆盖状态。

`wm density` 的输出格式相同（`Physical density` / `Override density`）。

## 三、恢复命令

```bash
adb shell wm size reset
adb shell wm density reset
```

执行完立即回到系统默认。这两条正是 Screener 官方仓库给出的恢复方法。同族的还有一条，显示区域（画面四周被裁掉）异常时用：

```bash
adb shell wm overscan reset
```

## 四、连不上手机时的排查

1. 「USB 调试」没开就连不上——这个开关必须事先打开。黑屏状态下如果从来没开过 USB 调试，ADB 这条路走不通，只能依赖 10 秒自动撤销（见 [黑屏恢复与常见问题.md](黑屏恢复与常见问题.md)）；
2. `adb devices` 显示 `unauthorized`：在手机弹出的「是否允许调试」里勾选「总是允许」；一直授权不上，重启手机与电脑后再试；
3. 同一台电脑连了多台设备时，给命令加序列号：`adb -s <序列号> shell wm size reset`，序列号就是 `adb devices` 列表里 `device` 前面那串字符。

## 五、两个使用建议

- 有玩家文档建议**重启设备前先恢复默认分辨率**：带着覆盖值重启，部分设备可能出现布局错乱、触控错位、应用闪退等异常（因设备而定）；
- 个别应用在恢复分辨率后布局仍不对，重启那个应用或重启设备即可。
