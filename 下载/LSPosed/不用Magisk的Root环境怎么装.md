# LSPosed 不用Magisk的Root环境怎么装

> 本篇讲 KernelSU 与 APatch 这两条路线怎么把框架装起来，以及它们和 Magisk 路线容易搞混的地方。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [模块装了不生效怎么排查.md](模块装了不生效怎么排查.md) · [同名项目与官方渠道怎么认.md](同名项目与官方渠道怎么认.md)

---

> [!IMPORTANT]
> **LSPosed 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/293a930cd5dc](https://pan.quark.cn/s/293a930cd5dc)

---

官方 README 里写的安装前提是「Magisk v24+」，但现在的 Root 方案不止 Magisk 一种，KernelSU 与 APatch 的用户越来越多。这两条路线同样能跑 LSPosed，只是中间要多一层：它们本身不含 Zygisk，需要额外装一个提供 Zygisk 环境的模块。本篇把这条链路补上。

## 一、先确认你用的是哪一套 Root

| 你的 Root 方案 | 自带 Zygisk 吗 | 装 LSPosed 前还要做什么 |
| --- | --- | --- |
| Magisk | 自带（设置里打开即可） | 无，直接装框架包 |
| KernelSU | 不含 | 先装一个提供 Zygisk 环境的模块 |
| APatch | 不含 | 同上 |

判断自己用的是哪一套很简单：看手机上那个授权管理应用叫什么、以及它的模块页长得什么样。三者的模块页都能装 zip，但模块目录与加载方式不同。

## 二、KernelSU 路线

KernelSU 的官方常见问题里明确写了：它本体不支持 Zygisk，要用 Zygisk 模块得靠第三方模块来提供环境，LSPosed 也正是靠这个环境才能运行。

1. **装 Zygisk 环境提供者**：在 KernelSU 管理器里，像装普通模块一样装上它的 zip，然后重启。最早、覆盖面最广的是 **ZygiskNext**；社区里还有 ReZygisk、NeoZygisk 这类替代实现，它们只保证最基本的 Zygisk 接口，部分特性不支持——选哪个按你手上模块的说明来，拿不准就先用 ZygiskNext。
2. **装 LSPosed 框架包**：重启后确认 Zygisk 环境已生效，再在 KernelSU 管理器里安装 `zygisk-release.zip` 那一支，重启。
3. **确认结果**：重启后状态栏的通知与管理器首页的状态，跟 Magisk 路线完全一样，判断标准见 [下载与安装教程.md](下载与安装教程.md) 第五节。

两个容易漏的前提：

- 模块包**只用 zygisk 版**。KernelSU 上没有 Riru，装 riru 版不会报错，但框架起不来；
- KernelSU 上如果模块需要改动 `/system` 下的文件，还要装一个 metamodule（例如 meta-overlayfs）来做挂载。LSPosed 走的是 Zygisk 注入这条路，不属于这一类，不需要额外装。

## 三、APatch 路线

APatch 与 KernelSU 在这个问题上的结论一样：本身不提供 Zygisk，需要 Zygisk 环境提供者。装法与第二节完全相同——先装提供者、重启，再装 LSPosed 的 zygisk 版包、重启。

需要额外知道两点：

- APatch 有它自己的权限机制（模块与应用授权走一套独立的凭据），第一次装模块时如果被拦，按管理器的提示授权即可；
- APatch 的模块体系继承自 KernelSU，为 KernelSU 写的模块在 APatch 上一般能直接装。

## 四、两套 Root 别叠着用

一个常见做法是「已经装了 Magisk，又想试试 KernelSU」，两个一起上。这里有个坑要提前知道：

- KernelSU 的模块系统与 Magisk 的挂载方式冲突——**只要 KernelSU 里启用了任何模块，Magisk 就会完全不工作**；
- 只有一种情况两者能共存：KernelSU 只用它的 root 授权功能、不启用任何模块，Magisk 照常改 ramdisk。

所以对 LSPosed 来说，**选定一套 Root 方案并用它装完框架，不要两边各装一份**。两边各装一份的表现不是报错，而是其中一边的框架状态永远是未激活。

## 五、装完最容易卡住的三处

| 现象 | 原因 | 处理 |
| --- | --- | --- |
| 管理器一直显示未激活 | Zygisk 环境提供者没装、或没重启生效 | 按第二节顺序重来一次，每装一个模块都重启 |
| 模块列表在管理器里不出现 | 装的是 riru 版包 | 换成 zygisk 版 |
| 框架能用，但个别模块报找不到环境 | 该模块需要提供者支持的某个特性，而替代实现没有 | 换成 ZygiskNext，或按模块说明降级模块版本 |

更多「装了没反应」的情形与排查顺序，整理在 [模块装了不生效怎么排查.md](模块装了不生效怎么排查.md)。

## 六、卸载与救援的对应关系

卸载时，这两个模块是**分开的**：先在管理器里移除 LSPosed，再移除 Zygisk 环境提供者，然后重启。只移除前者的话，Zygisk 环境还在，只是不再有框架去用它。

真遇到开不了机，KernelSU 与 APatch 的救援入口和 Magisk 不一样，能用命令行处理的场合也多一条路，具体命令写在 [开机卡住进不去系统怎么救.md](开机卡住进不去系统怎么救.md) 第二节。KernelSU 自带安全模式（开机第一个画面之后连续按音量减三次），进安全模式后所有模块会被停用，这是比刷 Recovery 更省事的一条退路。
