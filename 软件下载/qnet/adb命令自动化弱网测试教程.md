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

**参数传递方式**：`--ei` 表示参数值是 int，`--es` 表示参数值是字符串。各参数的含义对照 [弱网模板参数设置教程.md](弱网模板参数设置教程.md) 里的表格理解。

## 三、官方参数详表

官方 2.0 手册给出的完整参数列表（照表填值；「默认值」是不传时的取值）：

| 参数 | 键值 | 类型 | 默认值 | 说明 |
| --- | --- | --- | --- | --- |
| 执行命令 | `command` | 字符串 | 无 | `update` 更新参数；`stop_service` 结束进程。**更新和停止命令必须包含它** |
| 上行带宽 | `out_bandwidth` | int | -1 | 单位 kbps |
| 下行带宽 | `in_bandwidth` | int | -1 | 单位 kbps |
| 上行延时 | `out_delay` | int | -1 | 单位 ms |
| 上行延时抖动 | `out_delaybias` | int | -1 | 单位 ms |
| 下行延时 | `in_delay` | int | -1 | 单位 ms |
| 下行延时抖动 | `in_delaybias` | int | -1 | 单位 ms |
| 上行随机丢包 | `out_rate` | int | -1 | 0%~100% |
| 上行连续丢包（放行） | `out_pass` | int | -1 | 单位 ms，**需和丢包一起设置才生效** |
| 上行连续丢包（丢包） | `out_loss` | int | -1 | 单位 ms，**需和放行一起设置才生效** |
| 上行连续丢包（时间点发送） | `out_burst` | int | -1 | 单位 ms，需和放行一起设置才生效 |
| 下行随机丢包 | `in_rate` | int | -1 | 0%~100% |
| 下行连续丢包（放行） | `in_pass` | int | -1 | 单位 ms，需和丢包一起设置才生效 |
| 下行连续丢包（完全丢包） | `in_loss` | int | -1 | 单位 ms，需和放行一起设置才生效 |
| 下行连续丢包（Burst） | `in_burst` | int | -1 | 单位 ms，需和放行一起设置才生效 |
| 协议控制 | `protocol` | int | 15 | 1 = TCP，2 = UDP，4 = DNS，8 = ICMP，**按位或**关系 |
| 是否抓包 | `dump_pcap` | int | -1 | 1 启用，0 不启用；**只在启动命令时生效**；文件生成在 `/sdcard/Android/Data/com.tencent.qnet/cache/` |
| 控制应用包名 | `package_name` | 字符串 | 空 | 为空 = 全局生效，多个应用用竖线 `\|` 间隔；**只在启动命令时生效** |
| IP 列表 | `ip_list` | 字符串 | 空 | 为空 = 全局生效，多个 IP 用竖线 `\|` 间隔 |
| 信息悬浮窗开关 | `info_float_window` | int | -1 | 1 开启，0 关闭 |

表里几个值得单独提醒的点：

- **`protocol` 按位或**：想同时控 TCP 和 UDP 就传 `1 + 2 = 3`；TCP + UDP + DNS = `7`；全控 = `15`（也正是它的默认值）。前面示例里的 `protocol 3` 即 TCP/UDP 双协议。
- **连续丢包是三件套**：放行（`*_pass`）、丢包（`*_loss`）、Burst（`*_burst`）互相依赖，只传一个不生效 —— 官方明确「需要和放行/丢包一起设置才会生效」，要成组地传。
- **默认值 -1 基本等于「不设置」**：结合第三节的「全量更新」语义，`update` 时没传的参数会被设为默认值，而不是保留上一轮的值。
- **`package_name` / `dump_pcap` 只在启动命令里用**：启动后再 update 这两个不会补生效；要换被测应用或开抓包，重新走一遍启动。

## 四、三个必须知道的坑

1. **QNET 进程不能被清理，否则弱网会被关闭**。脚本跑完一轮、或测试中切后台被系统回收，弱网就停了 —— 自动化里要在循环中保活 QNET（或跑完立即重启它），别假设它一直在。
2. **更新参数是全量更新**：参数里没设置的项会被**直接重置为默认值**，不是「只改你提到的那个」。所以每次 update 要把该用例需要的所有参数一次带全，否则上一条指令设的延时可能悄悄被清掉 —— 这是自动化里最容易查不出的诡异现象。
3. **广播动作串 `qnet.boradcast.drive` 是官方拼写**（`boradcast` 不是 `broadcast`），照抄即可，别「顺手纠正」，改了指令不会生效。

## 五、一个最小自动化骨架

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

跑完之后回手机上看抓包文件（如果启动时带了 `dump_pcap`），导出与分析方式见 [悬浮窗控制与抓包导出教程.md](悬浮窗控制与抓包导出教程.md)；要给测试结论找数据，看生成的报告文件（[测试报告数据怎么看.md](测试报告数据怎么看.md)）。如果指令没反应，先排查登录态与权限，见 [常见问题与解决方法.md](常见问题与解决方法.md)。
