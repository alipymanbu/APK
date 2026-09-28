# MikuMikuPhoto 与 PC 版 MMD 的区别

> 搜「mikumikudance 安卓」找到的往往是 MikuMikuPhoto——本篇讲清它和 PC 版 MikuMikuDance 各是什么、能做什么、不能做什么。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [拍照合成使用步骤.md](拍照合成使用步骤.md) · [模型与姿势数据导入方法.md](模型与姿势数据导入方法.md)

---

> [!IMPORTANT]
> **MikuMikuPhoto 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ea11d24c131e](https://pan.quark.cn/s/ea11d24c131e)

---

## 一、为什么这两个名字总被混在一起

MMD 圈的资料大多围绕 PC 上的 MikuMikuDance 展开，而安卓上能读取 MMD 数据的应用不多，MikuMikuPhoto 是其中之一。它的安装包名、网盘文件夹名经常直接写作 mikumikudance，于是搜「mikumikudance 安卓版」的人点进来，往往以为自己找到了官方手机版——**MikuMikuPhoto 不是 PC 版 MMD 的官方手机版**，它是 PujoHead Soft 这个开发者的独立作品。

## 二、PC 版 MikuMikuDance 是什么

MikuMikuDance（圈子里简称 MMD）是免费的 3D 角色动画软件，在 Windows 上运行。你给它载入模型（`.pmd` / `.pmx`）和动作数据（`.vmd`），再调整镜头与灯光，输出的是角色唱跳的动画视频。围绕它形成了完整的配布生态：模型、动作、姿势数据（`.vpd`）大多出自这个圈子，圈内常用的资料站 VPVP wiki 维护着模型数据的汇总页（[w.atwiki.jp/vpvpwiki/pages/65.html](https://w.atwiki.jp/vpvpwiki/pages/65.html)，日语页面）。想真去下这些数据，[MMD模型与姿势去哪下载.md](MMD模型与姿势去哪下载.md) 汇总了渠道与规矩。

## 三、MikuMikuPhoto 是什么

MikuMikuPhoto 是安卓上的拍照合成应用。它读取 MMD 的模型数据与姿势数据，把 3D 角色叠进照片——现场取景拍一张，或用相册里已有的照片。它不做动画：没有时间轴、不编排动作序列，姿势是摆好的静帧，产出是一张张合成照片。

应用自带一批 Crypton 系角色（初音未来、镜音铃·连、KAITO、弱音白等）的模型与若干姿势，角色使用权基于 Piapro Character License；内置内容随版本不同，怎么补自己的数据见 [模型与姿势数据导入方法.md](模型与姿势数据导入方法.md)。

## 四、一张表看清分工

| | PC 版 MikuMikuDance | MikuMikuPhoto |
| --- | --- | --- |
| 平台 | Windows | Android |
| 产出 | 动画视频 | 合成照片 |
| 读模型 `.pmd` / `.pmx` | 是 | 是（`.pmx` 支持是 2.0.0 才加入的，早期版本以 `.pmd` 为准） |
| 读动作 `.vmd` | 是 | 官方说明只提到姿势 `.vpd` |
| 操作方式 | 键鼠 | 触屏手势 |

## 五、按需求对号入座

- 你想在手机上**和 3D 角色合影**、出静帧图 → MikuMikuPhoto 就是干这个的，从 [下载与安装教程.md](下载与安装教程.md) 开始；
- 你想做**让角色跳舞的动画视频** → MikuMikuPhoto 给不了，你需要 PC 上的 MMD，或者另找支持动作数据的安卓应用（那是另外的应用，不在本篇范围内）。

想先看看它实际怎么操作，直接翻 [拍照合成使用步骤.md](拍照合成使用步骤.md)。
