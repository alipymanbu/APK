# QNET adb 命令自动化弱网测试教程

> 想用脚本批量跑弱网、接进自动化回归的，看这篇：启动、热更新、停止三条指令与踩坑点。
> **相关文档**：[弱网模板参数设置教程.md](弱网模板参数设置教程.md) · [悬浮窗控制与抓包导出教程.md](悬浮窗控制与抓包导出教程.md) · [常见问题与解决方法.md](常见问题与解决方法.md)

---

> [!IMPORTANT]
> **qnet 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/4aba7244212e](https://pan.quark.cn/s/4aba7244212e)

---

## 一、为什么要用 adb 驱动

手动操作是「选应用 → 选模板 → 启动 → 观察 → 停止」，测一两轮没问题；但你要把 2G、3G、100% 丢包这些用例全过一遍，或者接进 CI 每晚跑回归，就得让脚本代替手指。QNET 的 Android 版支持 adb 命令驱动：**启动弱网、更新参数、停止弱网**都有对应指令，一个脚本就能遍历所有弱网用例。

前置条件：

1. 手机连上电脑、`adb devices` 能看到设备；
2. QNET 已安装并**登录过一次**（命令不会帮你完成登录）；
3. 悬浮窗、VPN 权限至少手动授过一次，避免首次弹窗卡住脚本。

## 二、三条核心指令

**1. 启动弱网** —— 通过 AdbStartActivity 拉起，参数直接跟在后面：

```bash
adb shell am start {--[类型] [key] [value]} com.tencent.qnet/.Component.AdbStartActivity
```

使用示例（针对微信做 UDP 上行 50ms 延时并抓包）：

```bash
adb shell am start --ei "dump_pcap" 1 --es "package_name" "com.tencent.mm" --ei "out_delay" 50 --ei "protocol" 2 com.tencent.qnet/.Component.AdbStartActivity
```

**2. 更新弱网参数** —— 用广播发给 QNET：

```bash
adb shell am broadcast -a "qnet.boradcast.drive" --include-stopped-packages {--[类型] [key] [value]} com.tencent.qnet
```

使用示例（更新成 TCP/UDP 100% 丢包）：

```bash
adb shell am broadcast -a "qnet.boradcast.drive" --include-stopped-packages --es "command" "update" --ei "in_rate" 100 --ei "out_rate" 100 --ei "protocol" 3 com.tencent.qnet
```

**3. 结束弱网**（连带退出进程）：

```bash
adb shell am broadcast -a "qnet.boradcast.drive" --include-stopped-packages --es "command" "stop_service" com.tencent.qnet
```

**参数传递方式**：`--ei` 表示参数值是 int，`--es` 表示参数值是字符串。参数 key 与界面上的弱网参数一一对应（如 `out_delay` 上行延时、`in_rate`/`out_rate` 丢包率、`protocol` 协议、`package_name` 被测包名、`dump_pcap` 抓包开关），各参数的含义对照 [弱网模板参数设置教程.md](弱网模板参数设置教程.md) 里的表格理解。

## 三、三个必须知道的坑

1. **QNET 进程不能被清理，否则弱网会被关闭**。脚本跑完一轮、或测试中切后台被系统回收，弱网就停了 —— 自动化里要在循环中保活 QNET（或跑完立即重启它），别假设它一直在。
2. **更新参数是全量更新**：参数里没设置的项会被**直接重置为默认值**，不是「只改你提到的那个」。所以每次 update 要把该用例需要的所有参数一次带全，否则上一条指令设的延时可能悄悄被清掉 —— 这是自动化里最容易查不出的诡异现象。
3. **广播动作串 `qnet.boradcast.drive` 是官方拼写**（`boradcast` 不是 `broadcast`），照抄即可，别「顺手纠正」，改了指令不会生效。

## 四、一个最小自动化骨架

把三条指令串起来的典型流程：

```bash
# 1. 启动：对指定应用施加弱网并抓包
adb shell am start --es "package_name" "com.example.app" --ei "out_delay" 200 --ei "dump_pcap" 1 com.tencent.qnet/.Component.AdbStartActivity

# 2. 跑你的被测脚本（monkey、UI 自动化、接口压测均可）
# ... 此处执行待验证的操作 ...

# 3. 热更新参数换下一轮用例（记得参数带全）
adb shell am broadcast -a "qnet.boradcast.drive" --include-stopped-packages --es "command" "update" --ei "in_rate" 5 --ei "out_rate" 5 com.tencent.qnet

# 4. 全部跑完，停止弱网
adb shell am broadcast -a "qnet.boradcast.drive" --include-stopped-packages --es "command" "stop_service" com.tencent.qnet
```

跑完之后回手机上看抓包文件（如果启动时带了 `dump_pcap`），导出与分析方式见 [悬浮窗控制与抓包导出教程.md](悬浮窗控制与抓包导出教程.md)。如果指令没反应，先排查登录态与权限，见 [常见问题与解决方法.md](常见问题与解决方法.md)。
