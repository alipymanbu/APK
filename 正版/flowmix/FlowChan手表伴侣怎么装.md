# FlowChan 手表伴侣怎么装

> Flowmix 的穿戴端配件：支持哪些手表/手环、两个版本分别怎么装、连不上时怎么处理。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [后台保活与重启失效排查.md](后台保活与重启失效排查.md) · [新手怎么调音.md](新手怎么调音.md)

---

> [!IMPORTANT]
> **Flowmix 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f2b90a749954](https://pan.quark.cn/s/f2b90a749954)

---

## 一、FlowChan 能干什么

FlowChan（Flowmix 伴侣）是装在手表/手环上的小工具，两个用途：**快速开关 Flowmix**、**切换 EQ 配置**——不用掏手机。前提是手机上的 Flowmix 正常运行（后台保活做得如何见[后台保活与重启失效排查.md](后台保活与重启失效排查.md)）。

## 二、先确认你的设备在不在名单里

FlowChan 分两个版本，按平台装不同的包：

**Wear OS 版**——适用于大多数 Wear OS 设备，官方列出的包括：

- Google Pixel Watch 系列
- Samsung Galaxy Watch 系列
- TicWatch 系列
- 其他运行 Wear OS 的手表（多数 Wear OS 2.0 及以上设备可以试装）

**Vela 版**——小米生态穿戴设备，官方支持名单：

| 系列 | 型号 |
|---|---|
| 小米手表 | S4 Sport、S4、S3、S1 Pro |
| 红米手表 | 6、5、4 |
| 小米手环 | 10、9 Pro、9、8 Pro |

屏幕形态方面，圆屏、方屏、胶囊屏都做了适配。名单会随版本更新变化，以官方[下载页](https://docs.flowmix.ykload.com/download.html) 当时显示为准。

## 三、两个版本怎么装

安装包都在官方[下载页](https://docs.flowmix.ykload.com/download.html) 的「Flowmix 伴侣」一节。

**Wear OS 版**（装的是 APK）：

1. 下载 Wear OS 版安装包；
2. 用 ADB 或 Wear OS 工具箱装到手表上——具体步骤搜「Wear OS 安装 APK 教程」，不同手表入口略有差异。

**Vela 版**（装的是 rpk）：

1. 下载 Vela 版安装包；
2. 用 AstroBox 或表盘自定义工具安装——小米手表/手环的第三方应用安装搜「小米手表安装第三方应用教程」。

## 四、首次配置与日常用法

1. 手机上打开 Flowmix；
2. 手表上打开 FlowChan；
3. 确认手机与穿戴设备已配对、蓝牙连着。

之后在表上点开关即可切换 Flowmix 启停，滑动列表换 EQ 配置，界面会显示当前生效的配置。

## 五、连不上、显示不全

**连不上 FlowChan**，按顺序试：

1. 手机与穿戴设备是否正确配对；
2. 手机上的 Flowmix 是否正在运行；
3. 到手机蓝牙设置里确认穿戴设备的连接状态；
4. 重启 FlowChan 或 Flowmix。

**界面显示不完整**：圆/方/胶囊屏都已适配，出现显示问题先升级到最新版再看。

**卸载**：Wear OS 设备在手表设置的应用管理里卸；小米穿戴设备用 AstroBox 或表盘自定义工具卸。

FlowChan 只管开关与切配置，音质本身怎么调在手机端，见[新手怎么调音.md](新手怎么调音.md)；手机端本体的安装在[下载与安装教程.md](下载与安装教程.md)。
