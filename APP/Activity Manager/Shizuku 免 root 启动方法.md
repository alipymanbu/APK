# Shizuku 免 root 启动方法

> 本篇讲不想 root 时怎么用 Shizuku 给 Activity Manager 授权，去启动那些被系统拦住的未导出页面。要求 Activity Manager 5.5.0 以上。
> **相关文档**：[启动隐藏 Activity 的方法.md](启动隐藏%20Activity%20的方法.md) · [下载与安装教程.md](下载与安装教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **Activity Manager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/dc74ec1165ee](https://pan.quark.cn/s/dc74ec1165ee)

---

## 一、Shizuku 是什么、为什么能免 root

Shizuku 是一款独立的授权工具：你先把它以 adb 或 root 权限跑起来，其他应用再通过它拿到接近 adb 的系统权限，全程不用刷机。Activity Manager 从 5.5.0 起支持它，所以第一步是先把 Activity Manager 更新到 5.5.0 以上（渠道见 [下载与安装教程.md](下载与安装教程.md)），老版本里没有这条路。

## 二、把 Shizuku 跑起来（三条路选一条）

Shizuku 应用本身从其官方站点 [shizuku.rikka.app](https://shizuku.rikka.app/) 指引的渠道获取。启动方式按官方手册（截至 2026-09）有三种：

| 方式 | 适用 | 要点 |
| --- | --- | --- |
| root 启动 | 已 root 的设备 | Shizuku 应用里直接启动，可设开机自启 |
| 无线调试 | Android 11 以上，免电脑 | 需先开开发者选项与 USB 调试；配对一次即可，每次重启后要重新点启动 |
| 连电脑 adb | 未 root 且系统较旧（Android 10 及以下） | 电脑装 Google 的 platform-tools，USB 调试连上后执行 Shizuku 给出的命令；重启后同样要重来 |

无线调试的具体步骤：

1. 系统设置里开启「开发者选项」（各品牌开启方式不同，按自己机型搜）；
2. 开发者选项里同时打开「USB 调试」和「无线调试」；
3. 在 Shizuku 里点「配对」，到系统「无线调试」页选「使用配对码配对设备」，把弹出的配对码填进 Shizuku 的通知；
4. 配对只需一次，之后回到 Shizuku 点「启动」即可；启动不了就把无线调试关掉再开一次。

连电脑的方式：电脑端解压 Google 的 SDK Platform Tools，手机开 USB 调试并连接，终端里 `adb devices` 出现设备后，把 Shizuku 应用里显示的那条启动命令原样粘贴执行。

## 三、把权限交给 Activity Manager

1. 确认 Shizuku 应用里显示「运行中」。
2. 打开 Activity Manager，触发 Shizuku 的授权弹窗并允许（入口在该版本的应用设置里，以界面实际选项为准）。
3. 回去点之前报 SecurityException 的未导出 Activity，能正常打开就说明授权成功。

## 四、官方手册列出的常见卡点

- 「一直在搜索配对服务」：给 Shizuku 后台运行权限，不少厂商系统会在应用退到后台后切断它的本地网络访问。
- 「输入配对码后立刻失败」：MIUI（小米/POCO）要把通知样式从「通知」切回「安卓」（系统设置 → 通知 → 通知栏）。
- 提示「adb 权限受限」：MIUI 在开发者选项里另开「USB 调试（安全设置）」（与普通 USB 调试是两个开关）；ColorOS 关闭「权限监测」；Flyme 关闭「Flyme 支付保护」。
- Shizuku 用着用着自己停了：保持它后台可运行、别关开发者选项，USB 用途改成「仅充电」。
- 厂商改版频繁，以上是官方手册口径，卡住时以 [shizuku.rikka.app](https://shizuku.rikka.app/) 的 FAQ 为准；与 Activity Manager 本身相关的其他问题另见 [常见问题与故障排查.md](常见问题与故障排查.md)。

## 五、嫌重启后要重开麻烦

- root 设备：Shizuku 支持开机自启，设置一次即可。
- 非 root 设备：这是系统限制，无线调试与 adb 两种方式重启后都得重跑一遍启动步骤；完全不想折腾就退回 root 方案，两种路线的取舍见 [启动隐藏 Activity 的方法.md](启动隐藏%20Activity%20的方法.md)。
