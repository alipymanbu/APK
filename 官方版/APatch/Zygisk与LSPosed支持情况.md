# Zygisk与LSPosed支持情况

> APatch 默认不带 Zygisk，LSPosed 因此不能直接用——官方列的几条加载路径与它们各自的代价。
> **相关文档**：[模块装不上和重启后丢授权怎么办.md](模块装不上和重启后丢授权怎么办.md) · [和Magisk与KernelSU的区别.md](和Magisk与KernelSU的区别.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **APatch 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/97a3e4130374](https://pan.quark.cn/s/97a3e4130374)

---

## 一、默认状态：没有 Zygisk

官方 FAQ 的原话是 **APatch 与 KernelSU 保持一致，默认不附带对 Zygisk 的支持**。装完 APatch 就直接找 Zygisk 模块来装，是装不上的——那些模块需要先有一个 Zygisk 环境。

要拿到这个环境，得额外装一个「提供 Zygisk 的 APM」，社区里常用的几个如下。

## 二、社区里的 Zygisk 实现（截至官方 FAQ 所列）

| APM | 状态 | 官方 FAQ 提到的要点 |
| --- | --- | --- |
| ZygiskNext | 活跃 | 最早为 KernelSU 提供 Zygisk 环境，对 Zygisk API 完整实现；**`0.9.1.1` 及之前是自由软件，之后为专有软件**；明确适配 APatch 的起始版本是 `1.0.3` |
| Zygisk_mod | 已归档 | ZygiskNext 适配 APatch 之前的过渡方案，适配完成后停止更新 |
| ReZygisk | 早期开发 | ZygiskNext 转专有后出现的自由实现，部分 ZygiskNext 的专有特性不支持 |
| NeoZygisk | 活跃 | 自由实现，只做最基本的 Zygisk API，设计直接衍生自 Magisk 内建 Zygisk |

**版本号会变、项目状态会变，装之前以各项目页面当时显示为准。** 官方对这四个的态度写得很清楚：你可以任选其一或用自己的实现，但 **APatch 不保证它们的可用性、适用性与稳定性**，遇到问题**不要直接向 APatch 提 issue，先找这些 APM 的作者**。

## 三、LSPosed 装不上的原因

LSPosed 依赖 Riru 或 Zygisk，而 APatch 默认两样都不带，所以**不能直接装**。官方给过两条路：

1. **先按上一节装一个 Zygisk 实现**，再走 LSPosed 的常规安装——这是官方文档里排在第一位的方案。
2. **Zloader 的 LSPosed 专版**，让 LSPosed 在没有 Zygisk 的情况下单独加载。官方对这条路已经明确劝退：**Zloader 在 `0.1.3` 之后没有新版本、也没有代码提交**，官方不再建议使用，让你改为引入 Zygisk。另外 Zloader 与 Zygisk 不兼容，两者只能留一个。

## 四、Shamiko 这类模块

官方 FAQ 的回答是：**Shamiko 是专有软件，APatch 无法适配**，并声明不对因使用它导致的任何问题负责，风险自负。

这条只说明兼容性现状——**装不上就是装不上，官方不提供适配，也不为此兜底**。

## 五、装完之后的验证

Zygisk 环境是否真的起来了，用依赖它的模块自己去验：能加载、模块页状态正常、重启后仍在，就算成了。授权异常（重启后自动放行或丢权限）是另一类问题，处理办法在 [模块装不上和重启后丢授权怎么办.md](模块装不上和重启后丢授权怎么办.md) 第三节。

要判断某个模块在 APatch 上「该不该能用」，先回到它对宿主环境的要求：只要它写明依赖 Zygisk，就必须先完成第二节那一步；只要它没写依赖，一般直接装，装不上再按报错码查，见 [模块装不上和重启后丢授权怎么办.md](模块装不上和重启后丢授权怎么办.md) 第一节。三种 Root 方案在模块体系上的差异见 [和Magisk与KernelSU的区别.md](和Magisk与KernelSU的区别.md)。
