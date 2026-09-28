# KernelSU 模块与Zygisk使用指南

> 模块怎么装、为什么改 /system 还要再装一个 metamodule、Zygisk 模块怎么跑起来，以及和 Magisk 模块那些不一样的细节。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [root权限授权与管理.md](root权限授权与管理.md) · [常见问题与救砖方法.md](常见问题与救砖方法.md)

---

> [!IMPORTANT]
> **KernelSU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/c98ba101d9d3](https://pan.quark.cn/s/c98ba101d9d3)

---

## 一、模块系统先说三件事

刚装好 KernelSU 的人最容易在这三件事上犯迷糊：

1. **模块是 zip 包**，在管理器的模块页面从本地存储选择安装，装完在 `/data/adb/modules` 目录下，这点和 Magisk 一样。
2. **不是所有模块都要额外装东西**。只用脚本（`post-fs-data.sh`、`service.sh`）、`sepolicy.rule`、`system.prop` 的模块，装了就能用。
3. **要改 /system 文件的模块，必须再装一个 metamodule**（例如官方的 `meta-overlayfs`），否则模块里的系统替换不生效。这是 KernelSU 与 Magisk 最大的结构差异：Magisk 把挂载逻辑内置在核心里，KernelSU 把挂载委托给可插拔的 metamodule。全新安装后「模块不工作」，先检查是不是漏了这一步。

metamodule 的使用要点（来源：官方 metamodule 指南）：

- **安装方式与普通模块完全一样**：拿到 `meta-overlayfs.zip` → 管理器模块页的悬浮按钮（➕）→ 选中 zip → 重启。`meta-overlayfs` 是官方参考实现，提供基于 overlayfs 的挂载；
- **当前生效的是哪个 metamodule**，在管理器模块页里能看到（有特殊标识的条目）；
- **同一时间只能装一个** metamodule，装第二个会被直接阻止。想换一个的话顺序是：卸载全部常规模块 → 卸载当前 metamodule → 重启 → 装新 metamodule → 重装常规模块 → 再重启；
- **卸载 metamodule 影响所有模块**：卸掉之后所有模块都不再挂载，直到装上下一个。管理器对这一步有单独的警告提示，别当成普通模块顺手卸了。

具体可用版本以官方模块指南（`https://kernelsu.org/zh_CN/guide/metamodule.html`）当时列出的为准。

## 二、装模块的完整动作

1. 把模块 zip 传到手机存储（或直接用手机下载）；
2. 打开 KernelSU 管理器 → 模块页 → 从存储安装，选中 zip；
3. 装完重启；
4. 重启后回模块页看开关状态，确认模块已启用；
5. 验证效果：改 /system 类模块要看目标文件是否真的变了，脚本类模块看脚本日志。

一个忠实的提醒：**不要刷来路不明的模块**。模块拥有 root 权限，恶意模块可以造成不可逆损坏——官方救砖文档把这条写在最前面不是吓唬人。

## 三、Zygisk 与 LSPosed

KernelSU 本体不支持 Zygisk，但官方 FAQ 给出的现成答案是用 [ZygiskNext](https://github.com/Dr-TSNG/ZygiskNext)：

1. 先按上文把模块系统装好；
2. 安装 ZygiskNext 模块并重启；
3. 之后就可以在 ZygiskNext 之上安装 Zygisk 模块，LSPosed 也依赖这条路径正常运行。

这条链路的每一环版本都可能更新，装不上或重启后不生效时，优先核对 ZygiskNext 与你 KernelSU 版本的兼容说明（项目页 README）。

## 四、想改 hosts 或改系统文件

- **改 hosts**（比如配合 AdAway 去广告列表）：KernelSU 没有内置这个能力，官方 FAQ 的答案是装 [systemless-hosts 模块](https://github.com/symbuzzer/systemless-hosts-KernelSU-module)；
- **把 /system 挂载为可读写**：官方明确不建议直接动系统分区，正规做法是用模块做 systemless 修改，也就是第一、二节说的 metamodule 路线；
- **删文件、替换文件**：KernelSU 模块不支持 Magisk 的 `.replace` 文件写法，改用 `mknod 文件名 c 0 0` 创建同名占位来删除对应文件，同时支持 `REPLACE` 与 `REMOVE` 变量。从 Magisk 迁移模块时这里是必改点。

## 五、从 Magisk 迁移过来的差异清单

如果你以前用 Magisk 模块，下面几条是 KernelSU 侧的差异（相同部分不赘述：zip 格式、安装目录、`post-fs-data.sh`/`service.sh` 时机、`system.prop`、`sepolicy.rule`、BusyBox 独立模式都一致）：

| 差异点 | KernelSU 的做法 |
| --- | --- |
| 挂载 /system | 需要装 metamodule（如 `meta-overlayfs`） |
| Recovery 里装模块 | 不支持 |
| Zygisk | 内置没有，走 ZygiskNext |
| `.replace` 文件 | 不支持，用 `mknod` 占位或 `REPLACE`/`REMOVE` 变量 |
| 新增脚本 | `boot-completed.sh`（开机完成后跑）、`post-mount.sh`（挂载完成后跑） |
| BusyBox 路径 | `/data/adb/ksu/bin/busybox`（内部行为，可能变） |
| 脚本里区分宿主 | 环境变量 `KSU` 在 KernelSU 下为 `true` |

## 六、与 Magisk 能不能共存

能，但有条件：**KernelSU 里启用了任何模块时，Magisk 的模块系统整体失效**——两者都在 `/data/adb/modules` 这层工作，模块挂载互相冲突。只使用 KernelSU 的 `su`、不启用模块时，两者可以同时工作（KernelSU 改 kernel、Magisk 改 ramdisk）。

所以打算共存的典型姿势是：root 授权交给 KernelSU 管，模块全交给其中一边——通常都收敛到 KernelSU 这边，Magisk 只保留 boot 补丁。两边都开模块是目前已知的冲突雷区。

## 七、模块装出问题先看救砖篇

模块引发的开机卡死是 KernelSU 用户最常遇到的故障，处理路径（音量键安全模式、ksud 禁用、Recovery 清理）单独成篇：[常见问题与救砖方法](常见问题与救砖方法.md)。给单个应用卸载模块造成的修改则归 App Profile 管，见[root权限授权与管理](root权限授权与管理.md)。
