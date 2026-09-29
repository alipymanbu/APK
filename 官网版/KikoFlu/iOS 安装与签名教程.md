# KikoFlu iOS 安装与签名教程

> iOS 版不是点开就能装的：官方发布的是未签名 IPA。本篇讲两条安装路线（AltStore / SideStore 与自签）各自的步骤和日常维护。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **KikoFlu 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/3cd0b1b27945](https://pan.quark.cn/s/3cd0b1b27945)

---

## 一、为什么不能直接安装

官方 Releases 页（`https://github.com/pa-jesusf/KikoFlu/releases`）给 iOS 提供的是**未签名 IPA**——这是普通开发者账号分发的常规形态，系统不允许直接安装，必须先经过「签名」。两条路：用 AltStore / SideStore 半自动管理，或自己签名。共同点是**都要一台电脑配合一次**。

## 二、路线一：AltStore / SideStore（推荐日常使用）

官方为这条路线提供了现成的软件源，装好后更新也不用再碰 IPA 文件：

1. 电脑上安装 AltServer（SideStore 则按其官网指引准备）；
2. iPhone 与电脑同一局域网，通过 AltServer 把 AltStore 装到手机上；
3. 打开手机上的 AltStore，添加 KikoFlu 官方软件源：
   `https://raw.githubusercontent.com/pa-jesusf/KikoFlu/main/altstore-source.json`
4. 在源列表里找到 KikoFlu 安装。

之后的**版本更新也在 AltStore / SideStore 里点一下即可**，不用重新下载 IPA。

## 三、路线二：自签

用自己的 Apple ID 给 IPA 签名后安装，适合不想装 AltStore 的人：

1. 从官方 Releases 页下载最新的未签名 IPA；
2. 选一个签名工具，按该工具自己的说明完成签名与安装（各工具操作差异大，以工具文档为准，这里不展开）；
3. 手机上信任对应的开发者证书（设置 → 通用 → VPN 与设备管理）。

**免费 Apple ID 签的证书有效期只有 7 天，到期要重新签**——这是 iOS 平台的机制，不是这个应用的问题；付费开发者账号（每年付费）证书有效期为一年。介意频繁重签的，走第二节那条路线。

## 四、装好之后

KikoFlu 装好后和安卓版一样是空播放端，第一件事仍是连服务器与登录，步骤见 [服务器连接与账号注册.md](服务器连接与账号注册.md)。iOS 的悬浮字幕走系统画中画（PiP）展示，播放相关功能与安卓版一致，见 [播放与字幕功能使用.md](播放与字幕功能使用.md)。

装不上、闪退、证书失效这类问题，[常见问题排查.md](常见问题排查.md) 的 iOS 一节有对应处理。
