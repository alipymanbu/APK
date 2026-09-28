# SecondScreen adb授权命令与常见报错

> 本篇讲不 root 的设备如何用 adb 给 SecondScreen 授予权限：环境准备、那一行命令、逐条报错的排查。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [分辨率与DPI配置教程.md](分辨率与DPI配置教程.md) · [黑屏或显示异常怎么恢复.md](黑屏或显示异常怎么恢复.md)

---

> [!IMPORTANT]
> **SecondScreen 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/4c7b13547109](https://pan.quark.cn/s/4c7b13547109)

---

## 一、为什么需要这一步

SecondScreen 改分辨率、改密度、关背光这些动作，本质上是在改系统显示设置，普通应用没有这个权限。官方的处理方式是：已 root 的设备直接给 root；未 root 的设备由你用电脑执行一条 adb 命令，把 `WRITE_SECURE_SETTINGS` 权限授予它。这条授权只需要做一次，做完后日常使用不需要再连电脑。

## 二、授权前的环境准备

按顺序做，每一步都可能卡住，别跳：

1. **手机端打开开发者选项**：设置 → 关于手机 → 连续点击「版本号」7 次（小米等品牌在「系统版本」或「OS 版本」上点，具体入口因品牌而异）。返回设置，会出现「开发者选项」。
2. **开启 USB 调试**：在开发者选项里打开「USB 调试」。
3. **电脑端安装 platform-tools**：从 Android 官方开发者网站下载 SDK Platform Tools（`https://developer.android.com/tools/releases/platform-tools`），解压到一个路径里没有中文和空格的目录，例如 `C:\platform-tools`。
4. **连接并确认**：手机用数据线连电脑，在解压目录里打开命令行窗口（Windows 可在资源管理器地址栏输入 `cmd` 回车），执行：

```bash
adb devices
```

手机上会弹出「是否允许 USB 调试」的确认框，勾选「一律允许」并确定。命令行里看到设备序列号和 `device` 字样，说明连接正常。

## 三、执行授权命令

连接正常后，执行这一行：

```bash
adb shell pm grant com.farmerbb.secondscreen.free android.permission.WRITE_SECURE_SETTINGS
```

命令没有任何输出就是成功。回到手机重新打开 SecondScreen，授权提示应该已经消失，可以进入配置文件环节了（见 [分辨率与DPI配置教程.md](分辨率与DPI配置教程.md)）。

## 四、常见报错逐条排查

| 报错 / 现象 | 原因 | 处理 |
| --- | --- | --- |
| `'adb' 不是内部或外部命令` | 命令行不在 platform-tools 目录 | `cd` 进解压目录再执行，或把目录加入 PATH |
| List of devices attached 为空 | 驱动没装好 / 数据线只能充电 / 没开 USB 调试 | 换原装数据线、装对应品牌驱动、核对第 2 步 |
| 显示 `unauthorized` | 手机上没点允许调试弹窗 | 拔插数据线，等弹窗出现后勾选允许 |
| `SecurityException: Must hold permission android.permission.WRITE_SECURE_SETTINGS` | 部分深度定制系统默认禁止 adb 授权 | 见下面第五节的品牌差异 |
| `Exception occurred while executing 'grant'` 且应用正在运行 | 应用进程在运行中，授予被拒 | 先执行 `adb shell am force-stop com.farmerbb.secondscreen.free` 再重新 grant |

执行 grant 报 SecurityException 是定制系统上最常见的一类问题，个别机型还需要多做一步：

## 五、定制系统的品牌差异

官方明确说过这款应用面向 AOSP / 原生体验类系统，在厂商定制系统上不保证正常。授权这一步的典型差异：

- **小米 / 红米（MIUI、HyperOS）**：开发者选项里有一个「USB 调试（安全设置）」开关，不开它，grant 命令会报 SecurityException。打开它通常还要求登录小米账号并插卡联网，开完建议重启一次手机再执行命令。
- **其他定制系统**：有的在开发者选项里提供「禁用权限监控」这类开关，作用类似。找不到对应开关的品牌，授权可能就是过不去，这属于系统层限制，换原生类设备或 root 是仅有的替代路径。

以上开关名称与位置随系统版本变动，以你设备当时的设置页为准。

## 六、授权之后：查看与收回

授权是持久的，但你有办法核对或撤销：

```bash
# 核对权限是否已授予（输出里找 WRITE_SECURE_SETTINGS）
adb shell dumpsys package com.farmerbb.secondscreen.free | findstr granted

# 收回授权
adb shell pm revoke com.farmerbb.secondscreen.free android.permission.WRITE_SECURE_SETTINGS

# 把该应用的所有运行时权限恢复到初始状态
adb shell pm reset-permissions com.farmerbb.secondscreen.free
```

收回后应用会回到「装了但什么都不做」的状态，想再用就重新执行第三节的 grant 命令。`findstr` 是 Windows 写法，macOS / Linux 终端里换成 `grep`。

Android 11 及以上的设备还有免数据线的无线调试路径，步骤单独整理在 [无线调试授权步骤.md](无线调试授权步骤.md)。授权完成后，如果后续使用中把显示参数改乱了，恢复办法见 [黑屏或显示异常怎么恢复.md](黑屏或显示异常怎么恢复.md)。
