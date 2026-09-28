# 常用ADB命令速查

> 本篇整理在手机上通过 iAdb 最常用得上的 ADB 命令，按用途分组，可直接照抄。
> **相关文档**：[授权文件管理器访问Android-data目录.md](授权文件管理器访问Android-data目录.md) · [无线调试开启与配对步骤.md](无线调试开启与配对步骤.md) · [常见问题与连接失败排查.md](常见问题与连接失败排查.md)

---

> [!IMPORTANT]
> **iAdb 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/243e096b19d8](https://pan.quark.cn/s/243e096b19d8)

---

## 一、用之前

1. 先按 [无线调试开启与配对步骤.md](无线调试开启与配对步骤.md) 把设备连上，`adb devices` 能看到设备再往下走；
2. 这些命令跑在 shell 权限下：能管应用、传文件，但改不了系统核心目录，Root 才能做的事这里做不了；
3. 命令里的 `包名` 换成目标应用的实际包名，不确定就先用第二节的命令查。

## 二、查设备与应用

```bash
adb devices                        # 列出已连接的设备
adb shell pm list packages         # 列出全部包名
adb shell pm list packages -3      # 只列第三方应用
adb shell pm list packages 关键词   # 按关键词过滤包名
adb shell pm path 包名             # 查某应用的 APK 安装路径
```

## 三、应用管理

```bash
adb shell am force-stop 包名       # 强制停止某应用
adb shell pm clear 包名            # 清除某应用的全部数据（慎用，等同恢复到首次安装状态）
adb shell pm grant 包名 权限名     # 给应用授予某项权限
```

`pm grant` 最常见的用途是授予 `android.permission.WRITE_SECURE_SETTINGS` 这类普通授权弹窗给不了的权限；具体该授哪个权限名，以目标应用的说明为准。

## 四、文件传输与查看

```bash
adb push 手机里的文件 目标路径        # 把文件推到对方设备
adb pull 对方路径 本地路径           # 把文件拉回当前手机
adb shell ls /sdcard/Android/data   # 列出受限目录内容（需已按授权流程授权）
adb install /sdcard/xxx.apk         # 把手机里存着的 APK 装到已连接的设备上
```

`adb install` 只吃整包 APK：XAPK / APKM 这类分包格式直接装会报错。给电视盒子装应用是这条命令最典型的场景，完整步骤见 [连接电视盒子的调试方法.md](连接电视盒子的调试方法.md)。

## 五、截屏与录屏

```bash
adb shell screencap -p /sdcard/screen.png   # 截屏并保存到设备
adb shell screenrecord /sdcard/demo.mp4     # 录屏（默认最长 3 分钟）
```

## 六、出错先看这三处

1. 命令回显 `not found` 或权限不足：设备可能已掉线，先重跑 `adb devices` 确认还在；设备名后面显示 `unauthorized` 时，去被调试设备的屏幕上把「允许 USB 调试」弹窗确认掉再重连；shell 权限做不了 Root 级操作；
2. 包名拼错：用 `adb shell pm list packages 关键词` 过滤确认后再执行；
3. 无线连接断断续续：两台设备离路由器太远，或路由器开了 AP 隔离，排查思路见 [常见问题与连接失败排查.md](常见问题与连接失败排查.md)。

更多命令的完整用法以 Android 官方开发者文档为准：[developer.android.com/tools/adb](https://developer.android.com/tools/adb)。
