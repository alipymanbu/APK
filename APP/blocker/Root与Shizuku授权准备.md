# 使用 Blocker 前的 Root 与 Shizuku 授权准备

> 本篇讲 Blocker 为什么需要高权限、四种控制方式的差别与各自适合的场景，以及 Root 和 Shizuku 两条路线的准备步骤。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [组件禁用操作指南.md](组件禁用操作指南.md) · [常见问题与报错处理.md](常见问题与报错处理.md)

---

> [!IMPORTANT]
> **Blocker 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/cae4ed240f23](https://pan.quark.cn/s/cae4ed240f23)

---

安卓系统只允许应用管理自己的组件；要改别的应用的组件状态，要么拿到 root 权限走命令行，要么借助 Shizuku/Sui 这类以 shell（或更高）权限启动的服务。Blocker 的开关能不能生效，取决于这篇的准备是否到位——应用装完先做这里，再去学操作。

## 一、为什么需要高权限

安卓的命令行工具 `pm` 可以禁用任意应用的组件（`pm disable 包名/组件名`），但它需要 root 权限才能运行，Blocker 的 Package Manager 控制方式走的就是这条路。Intent Firewall 与 Shizuku 则是另外两条通道，各有各的边界。

## 二、四种控制方式对照

你可以在 Blocker 的设置里切换控制器，几种方式之间可以无缝切换：

| 控制方式 | 依赖 | 改动效果 | 典型坑 |
| --- | --- | --- | --- |
| Package Manager（PM） | root | 直接改组件状态 | 应用若捕获组件启动异常，可能把组件再打开 |
| Intent Firewall（IFW） | root | 不改组件状态，只拦住启动它的意图 | 规则目录只有系统层能读写 |
| IFW + PM 组合 | root | 两种同时生效 | — |
| Shizuku / Sui | Shizuku 服务 | 走 shell 通道改组件状态 | shell 权限对未修改的普通应用常常不够（见第五节） |

两种主流方式的行为差异值得记住：PM 禁用后，应用再启动该组件会抛异常，开发者可以捕获异常把组件重新启用，所以你会看到「明明禁了又自己打开」；IFW 是防火墙性质的拦截，组件状态不变、应用检测不到被禁，也就不会自己恢复。组件自己弹回去怎么处理，见 [常见问题与报错处理.md](常见问题与报错处理.md)。

## 三、路线 A：已 root 的设备

1. 在你的 root 管理器（Magisk、KernelSU 等）里确认 Blocker 已获得 root 授权；
2. 打开 Blocker，控制器选 Package Manager 或 Intent Firewall 即可；
3. 想统一走 Shizuku 的话，直接在 Shizuku 里用「针对已 root 设备启动」。

root 路线最省事：权限完整，PM 与 IFW 两种控制器都能正常用。

## 四、路线 B：未 root，用 Shizuku

先装 Shizuku（官方页面：[shizuku.rikka.app](https://shizuku.rikka.app/)），再按你的系统版本选启动方式：

**无线调试启动（安卓 11 及以上，免电脑）**

1. 打开开发者选项与 USB 调试；
2. 进入「无线调试」，开启开关；
3. 在 Shizuku 里点「通过无线调试启动 → 配对」，再在系统的「无线调试 → 使用配对码配对设备」里拿到配对码，填进 Shizuku 的通知；
4. 回到 Shizuku 点「启动」。

由于系统限制，这种方式每次重启手机后都要重新做一遍启动。

**连接电脑启动（安卓 10 及以下）**

电脑上装好 adb（Google 提供的 SDK 平台工具）后执行：

```bash
adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh
```

**把 Shizuku 授权给 Blocker**

Shizuku 跑起来之后，打开 Blocker 的设置，把控制器切换到 Shizuku/Sui，在弹出的授权框里允许。

## 五、Shizuku 路线的关键限制

官方 FAQ 明确写过：对未做修改的普通应用，Shizuku 的 shell 权限不足以改变组件状态，点开关会报 `SecurityException: Shell cannot change component state`。要让 Shizuku 模式对普通应用生效，需要以 root 权限启动 Shizuku。也就是说，完全未 root 的设备上，Blocker 只能覆盖一部分场景；遇到这个报错不要反复点开关，先按 [常见问题与报错处理.md](常见问题与报错处理.md) 里的对应条目处理。

## 六、无线调试配对失败的排查

无 root 路线最常卡在配对这一步。Shizuku 官方手册把故障分了几类，对应处理：

- **一直显示「正在搜索配对服务」**：搜索配对服务需要访问本地网络，很多系统会在应用退到后台后立刻切断它的网络——先允许 Shizuku 后台运行（含后台联网），再重新开始配对；
- **点「输入配对码」后立刻提示失败**：MIUI/HyperOS 设备先把通知切回 Android 原生样式（系统设置的通知管理里切换）再试；另有用户反馈开发者选项里的「停用权限监控」开关也会影响配对；
- **adb 权限受限**：部分系统对无线调试的 adb 权限有额外限制，按官方手册对应条目处理；
- **启动失败**：把无线调试关掉再打开，重新走一遍启动（官方手册给的建议）。

两个实操细节：配对码页面要留在前台——离开「无线调试」页面配对会中断，可以用分屏让 Shizuku 通知和配对码页同屏；端口号每次开启无线调试都会变，重启手机后整套流程要重新做。更多机型特定的坑直接查官方手册：[shizuku.rikka.app/zh-hans/guide/setup](https://shizuku.rikka.app/zh-hans/guide/setup/)。

授权环境就绪后，接下来就是实际操作，见 [组件禁用操作指南.md](组件禁用操作指南.md)。
