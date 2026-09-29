# PeakVisor Garmin与Suunto设备连接

> 本篇讲怎么把 PeakVisor 里规划好的路线发到 Garmin 或 Suunto 手表：在哪里连接账号、为什么发不出去、路线发过去后在手表生态的哪里找。
> **相关文档**：[3D地图与路线规划.md](3D地图与路线规划.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **PeakVisor 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/2ab7d6eaafaf](https://pan.quark.cn/s/2ab7d6eaafaf)

---

## 一、先连接账号

官方口径里出现过两个连接入口，对应不同时期的版本，你手上是哪个版本就认哪个：

- 新版（2026 年 3 月官方公告口径）：**Profile → Settings → Devices** 里连接 Garmin 或 Suunto；
- 旧版（官方 FAQ 口径）：**Menu → Import → Import from Garmin / Import from Suunto** 登录授权。

两个前提别漏：Garmin 设备要先在 **Garmin Connect** 里完成配对；Suunto 要先装 Suunto 自家 App 并把手表配好。PeakVisor 侧的登录授权只是打通账号，代替不了设备配对。

## 二、发送路线的完整流程

官方 2026 年 3 月的公告把流程定成了五步：

1. 在 3D 地图上规划路线，或导入 GPX（做法见[3D地图与路线规划.md](3D地图与路线规划.md)）；
2. 查看地图、海拔剖面与路线数据；
3. **保存路线并设为公开**；
4. 打开 **Share** 对话框；
5. 选 **Export to Garmin** 或 **Export to Suunto**。

注意第 3 步：**只有公开（public）路线能导出到手表**，这是官方写明的限制，不是故障。私藏的路线先改公开再发；介意公开的话，导出完再改回去（以你手上版本提供的选项为准）。

发送成功后，路线会落到对应平台的这两个地方：

| 平台 | 路线出现在 | 常见找错的地方 |
| --- | --- | --- |
| Garmin | **Courses**（Garmin Connect 网页版在 Training & Planning → Courses；手机 App 在 Training/Courses） | 以为会出现在活动记录里 |
| Suunto | **Routes** | 同上 |

官方特别提醒过：发过去的是**导航路线，不是已完成的运动记录**，翻活动列表永远找不到。这算「发了却像没发生」的最常见误会。

## 三、Garmin 的活动同步

连接 Garmin 后还有个顺带的好处：Garmin 侧记录的户外活动会同步回 PeakVisor，能把走过的轨迹叠回它的 3D 地图上回看。反过来（PeakVisor 记录发到 Garmin）走的是上面第二节那条路线导出。

## 四、发不出去或收不到怎么排

按顺序过：

1. **发送按钮没有 Export to Garmin/Suunto** → 路线还不是公开状态，先按第二节第 3 步改公开；
2. **点发送报错或无反应** → 回连接入口（Profile → Settings → Devices，或 Menu → Import）**重新登录授权**一次，官方 FAQ 对「不同步」给的就是这个答案；
3. **发送成功了但手表上没有** → 先按第二节的落点去 Courses/Routes 里找；确认 Connect 里已有、设备上还是没有，就在设备侧手动同步（Garmin 设备：Menu → Connected Features → Phone → Sync Now），这是 Garmin 官方支持给出的通用做法；
4. 还不行 → 按[常见问题与故障排查.md](常见问题与故障排查.md)第六节的路子联系开发者，说明是哪一步卡住。
