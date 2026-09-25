# Taskbar adb 授权命令执行教程

> 桌面模式和小窗初始化都会让你在电脑上跑一两条 adb 命令，本篇把准备动作、命令清单和报错处理一次讲完。
> **相关文档**：[自由窗口小窗模式设置.md](自由窗口小窗模式设置.md) · [桌面模式开启与外接显示器教程.md](桌面模式开启与外接显示器教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **Taskbar 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f03b302717c9](https://pan.quark.cn/s/f03b302717c9)

---

## 一、什么时候轮到你跑 adb

三类场景会用到，前两类是 Taskbar 自己的引导会提的：

| 场景 | 要执行什么 |
| --- | --- |
| 开桌面模式时 | 给应用授一项特殊设置权限（官方原话是「某些设置需要通过 adb 授予特殊权限」） |
| Android 8.0 / 8.1 / 9 首次开小窗时 | 按弹窗指示执行一条 adb shell 命令（一次性） |
| 系统侧没开自由窗口能力时 | 打开系统的 freeform 支持开关（下面第三节有通用命令） |

全程不需要 root，只需要一台电脑、一根数据线。

## 二、先把 adb 环境跑通

1. 手机上打开**开发者选项**：设置 → 关于手机 → 连续点「版本号」7 次（不同品牌路径略有差异）。
2. 回到 设置 → 系统 → 开发者选项，打开 **USB 调试**。
3. 电脑下载 Android 官方的 platform-tools（[developer.android.com/studio/releases/platform-tools](https://developer.android.com/studio/releases/platform-tools)），解压到任意目录。
4. 数据线连接手机，电脑终端里执行：

```bash
adb devices
```

- 列表里出现设备序列号 + `device` → 通了，往下走。
- 出现 `unauthorized` → 手机屏幕上正在弹「允许 USB 调试吗」，勾选「始终允许」后点允许。
- 列表是空的 → 换数据线/换 USB 口（很多线只能充电），台式机尽量插主板后面的口；仍不行就装一次该品牌的 USB 驱动。

## 三、命令清单

以下命令按应用内弹窗**给出的为准**；弹窗没给、而你想手动开系统侧支持时，再用通用的这几条。

**1. 打开系统自由窗口（freeform）支持**（开小窗前的系统侧开关，通用做法）：

```bash
adb shell settings put global enable_freeform_support 1
```

执行完重启设备再回 Taskbar 开小窗。想恢复原状，把末尾的 `1` 换成 `0` 再执行一次并重启。

**2. 给 Taskbar 授特殊设置权限**（桌面模式相关设置用）：

```bash
adb shell pm grant com.farmerbb.taskbar android.permission.WRITE_SECURE_SETTINGS
```

这条命令来自第三方教程的写法，官方 README 只笼统说「某些设置需通过 adb 授予特殊权限」而没列命令 —— 所以**以应用引导里给出的那条为准**，两者不一致时听引导的。

**3. 系统开发者选项里的配套开关**：部分机型还需要在 设置 → 开发者选项 里打开 **「强制将活动调整大小」（Force activities to be resizable）**，这是个图形开关，不用命令。

## 四、报错速查

| 现象 | 怎么处理 |
| --- | --- |
| `adb devices` 空列表 | 换线/换口、装驱动，见第二节第 4 步 |
| `unauthorized` | 手机上点掉 USB 调试授权弹窗 |
| `offline` | 拔线重插，或 `adb kill-server` 后重来 |
| 命令提示 `Unknown command` | 命令被换行截断了，整行复制、确认没有断行 |
| 授权后桌面模式仍不生效 | 先重启 Taskbar，再不行重启手机（设置项常常要重启才读到） |

跑通之后回 [桌面模式开启与外接显示器教程.md](桌面模式开启与外接显示器教程.md) 继续勾选引导，或回 [自由窗口小窗模式设置.md](自由窗口小窗模式设置.md) 开小窗。
