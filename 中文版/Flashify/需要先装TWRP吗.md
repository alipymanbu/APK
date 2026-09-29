# Flashify 需要先装 TWRP 吗

> 本篇回答一个最常见的疑问：用 Flashify 之前要不要先装 TWRP——答案是「看你要做什么」，本篇把两种情况、两条安装路径和装好 TWRP 后多出来的能力一次讲清。
> **相关文档**：[刷入boot与recovery镜像.md](刷入boot与recovery镜像.md) · [刷入zip包与刷机流程.md](刷入zip包与刷机流程.md) · [备份与恢复教程.md](备份与恢复教程.md)

---

> [!IMPORTANT]
> **Flashify 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f9008b3b7076](https://pan.quark.cn/s/f9008b3b7076)

---

## 一、直接回答：看你要做什么

| 你要做的事 | 需要已装 TWRP 吗 |
| --- | --- |
| 刷入 boot 镜像（换内核） | 不需要 |
| 刷入 recovery 镜像（装 TWRP 本身） | 不需要 |
| 刷入 zip 包（ROM、Gapps、补丁） | **需要**（TWRP / Philz / CWM 任一） |
| 完整 nandroid 备份与恢复 | **需要**（TWRP 或 Philz） |

简单说：只动镜像分区，Flashify 自己就够；要刷 zip 或做完整备份，得先有一个自定义 recovery 在手机上。

## 二、两者各是什么角色

这两个东西不是二选一的关系，而是分工：

- **Flashify** 是系统里运行的刷入工具：它替你把 img 写进对应分区、把 zip 送进 recovery 安装、管理备份与云端同步，省去手动重启进 recovery 的往返；
- **TWRP** 是 recovery 本体：一个独立于安卓系统的救援环境，刷 zip 的安装脚本、nandroid 备份的完整快照，实际都是在它里面执行的。

Flashify 不替代 TWRP，也不被 TWRP 替代——反过来，你甚至可以用 Flashify 去安装 TWRP，这就是下面第三节的内容。

## 三、第一次装 TWRP 的两条路

**路线 A：应用内直接下载**（列表里有你的机型时优先走这条）：

1. Flashify → **Flash** 标签 → **Recovery image**；
2. 在列表里找你的机型，选中后下载并刷入；
3. 列表按机型提供，没有你的机型就别选相近的，改走路线 B。

**路线 B：自己下载 img 手动刷**：

1. 到 TWRP 官方支持页 `twrp.me/Devices`，按厂商和型号找到你的设备（页面上列不出你机型的话，见第五节）；
2. 下载对应机型的 img 文件，传到手机存储；
3. 按[刷入boot与recovery镜像.md](刷入boot与recovery镜像.md)第五节的手动流程刷入。

**装完之后有一个很多人踩的坑**：部分机型在刷入自定义 recovery 后首次重启进系统时，会把 recovery 换回原厂的。所以刷完不要先回系统，直接重启进 recovery 确认一遍：界面在、能操作，才算装稳了。具体按键组合见[刷入boot与recovery镜像.md](刷入boot与recovery镜像.md)第八节。

## 四、装好 TWRP 后多出来的能力

有了 TWRP，下面这些事才做得了（平时手动进 TWRP 操作，部分也能从 Flashify 里发起）：

| 能力 | 说明 |
| --- | --- |
| 完整 nandroid 备份 | 勾选 Boot、System、Data、Vendor 等分区做整机快照，操作见[备份与恢复教程.md](备份与恢复教程.md) |
| 高级 Wipe | 按分区精细清除，比双清更可控 |
| 挂载存储 | 在 recovery 环境里访问手机存储和 SD 卡 |
| ADB sideload / 文件管理 | 电脑推送刷机包、recovery 里直接找文件 |

刷 zip 的完整流程（含 Wipe 选项怎么勾、多文件队列）见[刷入zip包与刷机流程.md](刷入zip包与刷机流程.md)。

## 五、找不到你机型的 TWRP 怎么办

两种情况两种处理：

1. **官方支持页没有你的机型**：说明 TWRP 官方没为它出适配包。去你的机型开发者社区（如 XDA 对应板块）找社区维护的非官方版本——这类包质量参差，认准有持续更新、多方确认的帖子；
2. **有非官方 TWRP 但想换口味**：OrangeFox、SHRP 等第三方 recovery 也覆盖部分机型，各自的支持列表以它们的官方页面为准。

装不上、刷完报错或重启后 recovery 丢失等问题，排查思路整理在[常见问题与失败处理.md](常见问题与失败处理.md)。
