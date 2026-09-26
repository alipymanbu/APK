# 用 Tasker 等自动化应用触发 ADB 命令教程

> IntentShell 让你在自动化应用里发一条广播，ShizuTools 就替你执行对应的 ADB 命令。
> **相关文档**：[Shizuku配对与启动教程.md](Shizuku配对与启动教程.md) · [常见问题与报错排查.md](常见问题与报错排查.md) · [多应用同时发声与独立音量控制.md](多应用同时发声与独立音量控制.md)

---

> [!IMPORTANT]
> **ShizuTools 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/436b57ffbc12](https://pan.quark.cn/s/436b57ffbc12)

---

## 一、这个菜单解决什么问题

LocalShell 是你手动敲一条命令；IntentShell 是**让别的应用替你敲** —— 你把命令和一把钥匙打包成一条广播 intent 发给 ShizuTools，它在 Shizuku 权限下执行。这样「连上耳机就开某功能」「到公司自动切设置」这类想法，就能在 Tasker、MacroDroid 这类自动化应用里完成，不用每次自己开 ShizuTools 点菜单。

前提是 Shizuku 服务持续在运行（见 [Shizuku配对与启动教程.md](Shizuku配对与启动教程.md)），服务一停命令就会失败。

## 二、intent 怎么填

在自动化应用里建一个「发送 intent / send intent」动作，参数照下表填（官方 wiki 口径）：

| 参数 | 值 |
| --- | --- |
| 目标类型（target） | `Broadcast` |
| intent action | `com.legendsayantan.adbtools.execute` |
| intent package | `com.legendsayantan.adbtools` |
| extra 字符串 `command` | 你要执行的 ADB 命令 |
| extra 字符串 `key` | ShizuTools 应用里那串 **32 位十六进制访问密钥**（带不带短横线都可以） |

密钥在 ShizuTools 的 IntentShell 界面里，点一下就能复制（提示原文「Key copied to clipboard」）。它是**每台设备唯一**的 —— 从别人那抄来的动作块，`key` 必须换成你自己这台机器的，否则命令会被拒。

## 三、三条会翻车的地方

1. **密钥错了会连坐**：官方设计是「用错一次访问密钥，所有命令锁定 5 分钟」（应用内提示原文：locks for 5 minutes ... to prevent brute force attempts）。所以动作块跑不通时，先停手核对 `key`，别连续重试把你自己锁在门外。
2. **Shizuku 必须一直活着**：广播发到了、服务停了，命令照样执行不了。走无线调试方式的，重启手机后要先把 Shizuku 启动起来。
3. **命令本身的风险自己担**：发出去的命令和你在 LocalShell 里手敲的权限一样。凡是涉及删系统组件、改系统设置的命令，先手敲验证过再挂进自动化，别让一个误触发的宏反复执行。

## 四、和 LocalShell 的分工

| | LocalShell | IntentShell |
| --- | --- | --- |
| 谁来执行 | 你自己手动输入 | 自动化应用广播触发 |
| 适合 | 临时验证一条命令 | 重复触发、场景联动 |
| 钥匙 | 不需要 | 必须带 `key` |

第一次建议先在 LocalShell 里把手敲命令跑通（见 [常见问题与报错排查.md](常见问题与报错排查.md) 第四节），确认输出符合预期，再搬进自动化应用。

想要更细的场景（比如联动音量控制），命令可以配合 [多应用同时发声与独立音量控制.md](多应用同时发声与独立音量控制.md) 里的菜单行为来设计。
