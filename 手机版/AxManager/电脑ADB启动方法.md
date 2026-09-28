# AxManager 电脑 ADB 启动方法

> Android 10 及以下、或无线调试不灵时的启动方式：电脑装 platform-tools，数据线连手机，执行一条启动命令。
> **相关文档**：[无线调试启动与配对教程.md](无线调试启动与配对教程.md) · [常见问题与启动失败排查.md](常见问题与启动失败排查.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **AxManager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e76a7ed9b163](https://pan.quark.cn/s/e76a7ed9b163)

---

## 一、什么时候用这个方法

- 系统是 Android 10 及以下：系统里没有「无线调试」开关，免电脑启动走不通。
- Android 11 及以上但配对反复失败，也可以改用这个方法。

## 二、电脑上装 ADB

1. 下载 Google 官方的 SDK Platform Tools（[developer.android.com/tools/releases/platform-tools](https://developer.android.com/tools/releases/platform-tools)），解压到任意文件夹。
2. 在解压出的文件夹里打开终端：
   - Windows 10/11：在资源管理器地址栏输入 `powershell` 回车即可。
   - macOS / Linux：打开终端，`cd` 进该目录。
3. 输入 `adb` 回车，看到一大段用法说明而不是「找不到命令」，就说明环境好了。
   - macOS / Linux 下命令要写成 `./adb`。

## 三、连接手机

1. 手机开发者选项里打开「USB 调试」。
2. 数据线连接电脑。
3. 终端里执行：

   ```powershell
   adb devices
   ```

4. 手机弹出「允许 USB 调试」时，勾选「一律允许使用这台计算机进行调试」再确定。
5. 再执行一次 `adb devices`，列表里出现设备且状态是 `device`，就说明连上了。

状态对照（`adb devices` 输出里设备名后面那个词）：

| 显示 | 含义 | 处理 |
| --- | --- | --- |
| `device` | 已授权，连接正常 | 直接进入下一步 |
| `unauthorized` | 手机上还没点「允许调试」 | 看手机屏幕，勾选「一律允许」并确定；弹窗已消失就拔插一次数据线 |
| `offline` 或列表为空 | 数据线 / USB 口 / 驱动的问题 | 换数据线（只能充电的线连不上）、换 USB 口；电脑上执行 `adb kill-server` 后重插再试 |

   ```powershell
   adb kill-server
   adb devices
   ```

## 四、启动 AxManager

1. 打开手机上的 AxManager，进入「通过连接电脑启动」，把界面上给出的启动命令完整复制。
2. 粘贴到电脑终端里回车执行。
3. 手机上显示启动成功即可，之后电脑上的终端窗口可以关掉。

启动命令以 AxManager 界面里给出的为准（不同版本命令会变），不要照抄网上搜来的旧命令。

## 五、有 Root 的设备

已 Root 的设备可以不连电脑，直接用命令以 Root 模式启动。注意：截至 v1.4.x，Root 模式只能通过命令启动，应用内没有一键开关——这是官方用户手册里明确写的。Root/Su 模式与 Shell/ADB 模式的工作路径不同（v1.4.3 的更新说明里提到过这一点），从 Root 模块迁移过来的脚本要注意路径差异。

## 六、启动失败怎么办

- 提示 adb 权限受限：这是 MIUI、ColorOS、Flyme 的系统限制，按 [常见问题与启动失败排查.md](常见问题与启动失败排查.md) 第四节的对应开关处理。
- `adb devices` 列表里没有设备：换一根数据线（很多线只能充电不能传数据）、换 USB 口、确认手机弹窗已授权。
- 连上但启动命令执行报错：确认复制的是 AxManager 界面里的完整命令，并且终端没有关掉重开过（重开后要重新 `adb devices` 授权）。
