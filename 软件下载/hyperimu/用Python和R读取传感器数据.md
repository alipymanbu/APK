# 用Python和R读取传感器数据

> 本篇给两个电脑端读数的路子：先用十几行 Python socket 验证链路通不通，再用现成的 R 包 HypeRIMU 做按名取值与画图分析。
> **相关文档**：[数据推流到电脑的连接设置.md](数据推流到电脑的连接设置.md) · [CSV文件保存位置与数据格式.md](CSV文件保存位置与数据格式.md) · [连接不上与收不到数据的解决办法.md](连接不上与收不到数据的解决办法.md)

---

> [!IMPORTANT]
> **HyperIMU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/abd8f0be37f0](https://pan.quark.cn/s/abd8f0be37f0)

---

## 一、先用裸 socket 验证链路

不装任何库，用 Python 标准库先确认「手机确实在发、电脑确实能收」。UDP 收包最小代码（Python 3）：

```python
import socket

UDP_IP = "0.0.0.0"     # 监听本机所有网卡
UDP_PORT = 2055        # 与手机端填的端口一致

sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind((UDP_IP, UDP_PORT))
while True:
    data, addr = sock.recvfrom(1024)
    print(addr, data.decode("utf-8", errors="ignore"))
```

跑起来后在手机端把协议改成 UDP、IP 填这台电脑、端口填 2055，点转轮。屏幕能刷出逗号分隔的数值，链路就是通的，剩下的只是解析问题；刷不出来就按[连接不上与收不到数据的解决办法.md](连接不上与收不到数据的解决办法.md)逐项排查。收 TCP（CSV 或 JSON）换成这样：

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.bind(("0.0.0.0", 2055))
s.listen(1)
conn, addr = s.accept()
with conn:
    while True:
        data = conn.recv(1024)
        if not data:
            break
        print(addr, data.decode("utf-8", errors="ignore"))
```

## 二、R 用户：HypeRIMU 包

社区里有一个专为 HyperIMU 数据写的 R 包 **HypeRIMU**（作者 Johannes Friedrich，见 [HypeRIMU 项目页](https://johannesfriedrich.github.io/HypeRIMU/)），支持读本地 CSV 和读 TCP 流两种方式，安装：

```r
install.packages("devtools")
devtools::install_github("JohannesFriedrich/HypeRIMU")
```

- **读本地 CSV**（该包推荐的方式）：把手机存的 CSV 拷到电脑后：

```r
library(HypeRIMU)
data <- execute_file("short_y_impulse.csv")
```

  文件里带时间戳列时，函数会自动识别并把 UNIX 时间转成 POSIXct；
- **读 TCP 流**：`data <- execute_TCP(port = 5555)`，手机端协议选 TCP、IP 填这台电脑、端口对应好；
- **按名取传感器**：`get_specificSensor(data, sensorName = "MPL_Accelerometer")`——走文件方式的优势是字段带传感器名，不用自己数第几列；
- 配合 ggplot2 按时间画各传感器的曲线，项目页有完整示例代码。

## 三、其他语言怎么办

数据本身是普通 CSV/JSON，任何语言都能直接读：按[CSV文件保存位置与数据格式.md](CSV文件保存位置与数据格式.md)的字段顺序，用 pandas（Python）、`read.csv`（R）或 Excel 打开都行；JSON 走 TCP 时按行取包再 `json.loads` 即可。要一套现成的实时收包封装，优先用官方的 HIMUServer，见[数据推流到电脑的连接设置.md](数据推流到电脑的连接设置.md)。
