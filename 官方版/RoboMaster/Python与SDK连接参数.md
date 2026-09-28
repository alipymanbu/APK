# RoboMaster Python 与 SDK 连接参数

> 本篇给想在 App 实验室之外做二次开发的人：机器人开放的接入方式和 IP、六个通信端口各管什么、第一条控制命令怎么发，以及官方 robomaster Python 包的基本用法。
> **相关文档**：[实验室编程入门](实验室编程入门.md) · [连接机器人与激活步骤](连接机器人与激活步骤.md) · [操控与对战玩法](操控与对战玩法.md)

---

> [!IMPORTANT]
> **RoboMaster 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/d8279f1b3f28](https://pan.quark.cn/s/d8279f1b3f28)

---

## 一、什么时候轮到 SDK

App 内的实验室（见 [实验室编程入门](实验室编程入门.md)）解决的是「在手机上写程序」；当你需要在电脑上做更复杂的控制——让机器人接入自己的 Python 脚本、和其他设备联动、处理视频流——就要用机器人开放的网络接口（SDK）。官方开发者指南（RoboMaster Developer Guide，[robomaster-dev.readthedocs.io](https://robomaster-dev.readthedocs.io/zh-cn/latest/)）收录了完整参数，本篇整理自该指南，以官方最新文档为准。

一点前提说明：这套明文 SDK 是官方明确写在 RoboMaster EP 上的扩展能力，S1 在 App 内同样支持 Python，但开放接口以官方文档为准——按 EP 文档连不上时，先确认你的机型在不在支持列表里。

## 二、四种接入方式与 IP 地址

| 接入方式 | 怎么连 | 机器人 IP |
| --- | --- | --- |
| Wi-Fi 直连 | 中控切直连模式，电脑连机器人 Wi-Fi（贴纸上的名称 + 8 位密码） | 固定 `192.168.2.1` |
| USB | 数据线接智能中控的 USB 口（电脑需支持 RNDIS） | 固定 `192.168.42.2` |
| Wi-Fi 组网 | 中控切组网模式，电脑与机器人接同一路由器 | 路由器动态分配，监听 IP 广播端口（40926）获取 |
| UART | 接运动控制器的 UART 口 | 无 IP，串口参数：波特率 115200、8 数据位、1 停止位、无校验 |

UART 只出控制命令、消息推送、事件上报三类数据，**拿不到视频流和音频流**——要图传必须走 Wi-Fi 或 USB。

## 三、六个端口各管什么

| 数据类型 | 端口号 | 协议 | 说明 |
| --- | --- | --- | --- |
| 视频流 | 40921 | TCP | 需先发命令开启推送，才有数据输出 |
| 音频流 | 40922 | TCP | 需先发命令开启推送，才有数据输出 |
| 控制命令 | 40923 | TCP | 入口端口，通过它使能 SDK 模式 |
| 消息推送 | 40924 | UDP | 需先发命令开启推送，才有数据输出 |
| 事件上报 | 40925 | TCP | 需先发命令开启推送，才有数据输出 |
| IP 广播 | 40926 | UDP | 组网模式下用监听它拿机器人 IP |

第一步永远是连 **40923 控制命令端口**——SDK 模式从这里使能，其他端口都要在 SDK 模式开启后才工作。

## 四、第一条命令怎么发

官方示例的开发环境是一台有 Wi-Fi 的电脑加 Python 3.x。流程：

1. 中控切直连模式，电脑连上机器人 Wi-Fi；
2. 建立 TCP 连接到 `192.168.2.1:40923`；
3. 发送 `command`（命令以分号结尾），机器人返回 `ok` 即已进入 SDK 模式。

对应官方示例的核心代码结构：

```python
import socket

host = "192.168.2.1"   # 直连模式下机器人默认 IP
port = 40923           # 控制命令端口

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((host, port))

while True:
    msg = input(">>> please input SDK cmd: ")
    if msg.upper() == 'Q':
        break
    s.send((msg + ';').encode('utf-8'))   # 官方协议：命令以分号结尾
    buf = s.recv(1024)
    print(buf.decode('utf-8'))

s.shutdown(socket.SHUT_WR)
s.close()
```

发 `command` 得到 `ok` 之后，就可以按开发者指南里的协议表发各类控制命令了。

## 五、robomaster Python 包

不想手写 socket 的话，官方提供了 `robomaster` Python 包（pip 安装），把连接和各模块接口都封装好了。官方文档给出的基本骨架：

```python
from robomaster import robot

ep_robot = robot.Robot()
ep_robot.initialize(conn_type="sta")   # sta = 组网模式；默认为 Wi-Fi 直连
# ... 通过 ep_robot.chassis / ep_robot.gimbal 等模块对象做控制 ...
ep_robot.close()                       # 官方提醒：程序结束必须调用 close()
```

两个官方文档点名的注意点：

- 电脑有**多张网卡**时，SDK 自动获取的本机 IP 可能不对，需要手动指定 `robomaster.config.LOCAL_IP_STR`；
- 指南的能力章节覆盖多机通信、自定义 UI、发射器、拓展机构、视觉智能、装甲板、传感器、转接模块、UART——多数是 EP 的扩展硬件能力。

## 六、边界

本篇内容只用于连接你自己的机器人做开发学习；连接参数和命令协议会随固件与文档版本变化，动手前对照官方开发者指南的最新版（[robomaster-dev.readthedocs.io](https://robomaster-dev.readthedocs.io/zh-cn/latest/)）。
