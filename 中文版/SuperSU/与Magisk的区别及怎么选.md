# SuperSU 与 Magisk 的区别及怎么选

> 本篇把 SuperSU 和 Magisk 摆在一起对比：两者的现状、安装与原理口径、OTA 与隐藏能力差异，帮你判断手上的设备该用哪个，以及从 SuperSU 换到 Magisk 的正确顺序。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [root权限授权与日常管理.md](root权限授权与日常管理.md) · [完整卸载与取消root方法.md](完整卸载与取消root方法.md)

---

> [!IMPORTANT]
> **SuperSU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ae57de1aba73](https://pan.quark.cn/s/ae57de1aba73)

---

## 一、一句话结论

**老设备、老 ROM、跟着老教程走 → SuperSU 依然好用；Android 8/10 以后的新设备、要 OTA、要模块 → 直接 Magisk。** 两者不是竞品关系，更像前后两个时代的接力。

## 二、差异对比

| 维度 | SuperSU | Magisk |
| --- | --- | --- |
| 开发状态 | 2017 年停止更新（2.82 / SR5 为最后版本），2018 年从 Google Play 下架 | 持续活跃维护 |
| 开源情况 | 闭源 | 开源 |
| 作者 | Chainfire（后转手 CCMT） | topjohnwu |
| 安装方式 | TWRP 卡刷 ZIP，或已 root 设备直接装 APK | 应用内修补 boot 镜像，再 fastboot 刷入 |
| 对系统的改动 | 系统层植入 su（新版支持 systemless 模式） | systemless：不动系统分区，只改 boot |
| OTA 系统更新 | 多数情况要先取消 root 再更新 | 可保留（更新前还原、更新后再注入） |
| 模块生态 | 无 | 有，大量功能靠模块扩展 |
| root 检测应对 | 基本没有内置方案 | DenyList 及配套模块做隐藏 |
| 适合的系统版本 | 约 Android 2.3 ~ 9 | 约 Android 6.0 起新老通吃 |

## 三、什么情况下 SuperSU 仍是合理选择

- **老机型、老 ROM**：官方系统停在 Android 7/8 的设备，TWRP + SuperSU ZIP 的流程成熟、教程遍地；
- **照着老教程做**：不少机型论坛的刷机教程就是按 SuperSU 写的，照做最不容易出岔子；
- **对 root 需求很轻**：只是偶尔卸个预装、备份个应用数据，SuperSU 的免费功能完全够，也用不上模块。

安装路径见 [下载与安装教程.md](下载与安装教程.md)，日常授权操作见 [root权限授权与日常管理.md](root权限授权与日常管理.md)。

## 四、什么情况直接上 Magisk

- **Android 10 及以后**的设备，SuperSU 基本没有支持保障；
- **银行、支付类应用**是刚需——root 检测的应对是 Magisk 的强项；
- **要跟官方 OTA**，不想每次更新都重刷一遍；
- **需要模块**（去广告、音效、字体这类系统级改造）。

## 五、从 SuperSU 迁到 Magisk 的顺序

1. 在 SuperSU 里执行 **Full unroot**，重启确认 root 已清干净（步骤见 [完整卸载与取消root方法.md](完整卸载与取消root方法.md)）；
2. 装好 Magisk 应用，按它的流程修补当前系统的 boot 镜像并刷入；
3. 重启后在 Magisk 里确认"已安装"，之前由 SuperSU 管理的 root 应用重新走一遍授权。

两套方案不共存：顺序颠倒会出现 su 二进制和 Magisk 互相打架的中间态，务必先把旧的清干净，再装新的。
