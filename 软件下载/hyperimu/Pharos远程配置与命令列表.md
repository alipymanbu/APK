# HyperIMU Pharos远程配置与命令列表

> 本篇讲 Pharos 功能：让电脑通过 TCP 文本命令远程配置手机端 HyperIMU——改协议、改端口、开关传感器、启动停止采集，不用每次走到手机跟前点界面。
> **相关文档**：[数据推流到电脑的连接设置.md](数据推流到电脑的连接设置.md) · [连接不上与收不到数据的解决办法.md](连接不上与收不到数据的解决办法.md)

---

> [!IMPORTANT]
> **HyperIMU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/abd8f0be37f0](https://pan.quark.cn/s/abd8f0be37f0)

---

## 一、Pharos 是什么、怎么开

Pharos 让手机端反过来充当一个小服务端：你从电脑连上它约定的端口，发文本命令，就能远程修改 HyperIMU 的配置、控制采集的开始与停止。

开启路径：抽屉菜单 → **Pharos**：

- **Enable Pharos Server**：总开关，勾上后手机才开始接受命令连接；
- **Service port**：Pharos 监听的端口。注意它和数据推流的端口是两回事，各管各的；
- **Power Optimization**：省电优化。开启后手机休眠时可能不响应命令——要远程控制就别开它，或让手机保持不休眠。

## 二、先进入 Host Mode

配置类命令只在 **Host Mode** 下生效，它是一道防误操作的锁：

1. 连上 Pharos 端口后，先发 `enter host`，收到 `ACK` 才拿到配置权；
2. 同一时刻只允许一个客户端占用 Host Mode，被别人占着时你会收到 `NACK`；
3. 改完配置、要启动采集前，发 `exit host` 退出——运行 HyperIMU 与占用 Host Mode 是互斥的，`start`/`launch` 之前必须先退出；
4. 命令不区分大小写；每条命令的应答只有两种：`ACK`（已执行）、`NACK`（命令错误或不允许）。

## 三、常用命令一览

| 命令 | 作用 |
| --- | --- |
| `enter host` / `exit host` | 进入 / 退出配置模式 |
| `launch` | 只打开主界面，不开始采集 |
| `start` / `stop` | 打开并启动采集 / 停止采集 |
| `set protocol <值>` | 设置传输协议，值取 TCP / UDP / JSON / FILE / NONE |
| `get protocol` | 查询当前协议 |
| `set ip <地址>` / `get ip` | 设置 / 查询推流目标的服务端 IP |
| `set port <端口>` / `get port` | 设置 / 查询推流端口 |
| `set dt <毫秒>` / `get dt` | 设置 / 查询采样间隔 |
| `enable timestamp TRUE/FALSE` | 包头时间戳开关 |
| `enable MAC TRUE/FALSE` | 包头 MAC 地址开关 |
| `enable BATT TRUE/FALSE` | 包头电量信息开关 |
| `enable GPS TRUE/FALSE` | 包尾 GPS 开关 |
| `enable NMEA TRUE/FALSE` | 包尾 NMEA 开关 |
| `enable persistent TRUE/FALSE` | 断线重连开关 |
| `get names` | 列出设备上全部传感器名称（CSV 串） |
| `get types` | 列出全部传感器的类型码（CSV 串） |
| `set list <0/1串>` | 按顺序设置每个传感器是否纳入推流：1 收、0 不收；串可以比传感器总数短 |
| `get list` | 查询当前各传感器的纳收情况 |

## 四、从电脑发命令的最小流程

任何能发 TCP 文本的工具都行（Python socket、netcat、网络调试助手均可），流程固定为：

```text
连接 手机IP:Pharos端口
→ enter host            （等 ACK）
→ set protocol UDP
→ set ip 192.168.1.10
→ set port 2055
→ exit host             （等 ACK）
→ start                 （开始采集）
……
→ stop                  （结束采集）
```

`set ip`、`set port` 改的是数据推流的目标地址，电脑端对应的收包配置见[数据推流到电脑的连接设置.md](数据推流到电脑的连接设置.md)。连不上 Pharos 端口时，先确认 Enable Pharos Server 已勾选、两边在同一局域网，再按[连接不上与收不到数据的解决办法.md](连接不上与收不到数据的解决办法.md)的思路排查防火墙与端口。
