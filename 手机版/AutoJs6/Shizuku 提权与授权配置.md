# AutoJs6 Shizuku 提权与授权配置

> Shizuku 是一套独立的授权工具（不是 AutoJs6 的组成部分），本篇讲怎么装上并启动它、把权限交给 AutoJs6 用，以及重启手机后服务掉了怎么恢复。
> **相关文档**：[无障碍权限开启教程.md](无障碍权限开启教程.md) · [连接电脑开发教程.md](连接电脑开发教程.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **AutoJs6 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/80cb09f7f013](https://pan.quark.cn/s/80cb09f7f013)

---

## 一、Shizuku 能帮上什么忙

AutoJs6 支持通过 Shizuku 获得 ADB 特权并使用部分系统 API（官方说明），从 6.4.0 起内置了 `shizuku` 模块，脚本里可以直接调用，用法见官方文档站的 [shizuku 模块章节](https://docs.autojs6.com/#/shizuku)。

关系可以这样理解：Shizuku 这个应用自己持有一份 ADB（或 root）级别的权限，再把权限「转借」给你信任的应用（这里是 AutoJs6）。它不是 root，在未 root 的手机上它跑在 ADB shell 权限下，能做的事以这个身份为上限。

## 二、先把 Shizuku 装上

从官方渠道获取：官网 [shizuku.rikka.app](https://shizuku.rikka.app/)、Google Play，或项目在 GitHub 的 Releases 页。装好后先不要急着授权应用，Shizuku 自己还没「启动」。

## 三、启动 Shizuku 的三种方式

| 方式 | 适用设备 | 特点 |
| --- | --- | --- |
| 无线调试启动 | Android 11 及以上 | 不用电脑；配对只需一次；每次重启后要重新启动服务 |
| 连接电脑启动（ADB） | 未 root 且 Android 10 及以下 | 需要电脑和数据线；重启后同样要重来 |
| root 启动 | 已 root 设备 | 直接启动 |

### 无线调试启动（Android 11 及以上，推荐）

1. 开启开发者选项（一般在「关于手机」里连点「版本号」多次，路径随机型不同），并同时开启「USB 调试」；
2. 进入开发者选项里的「无线调试」，打开开关；
3. 打开 Shizuku，选择「通过无线调试启动」并开始配对；
4. 回到系统的「无线调试」页面，点「使用配对码配对设备」，把弹出的配对码填进 Shizuku 的通知输入框；
5. 配对成功后（配对只需做一次），在 Shizuku 里点「启动」。

### 连接电脑启动（Android 10 及以下或无线配对走不通）

需要电脑装好 platform-tools（ADB），手机开 USB 调试后连电脑，在电脑终端按 Shizuku 官方手册执行启动命令，命令以官方手册当时显示为准：[shizuku.rikka.app/guide/setup](https://shizuku.rikka.app/guide/setup/)。这套 ADB 环境和 [连接电脑开发教程.md](连接电脑开发教程.md) 里服务端模式要装的是同一套。

## 四、把权限交给 AutoJs6

Shizuku 跑起来之后，回到 AutoJs6 调用 Shizuku 能力时，系统会弹出 Shizuku 的授权确认框，允许即可。之后脚本里通过 `shizuku` 模块用到的特权能力就走这条通道。

## 五、重启手机之后会发生什么

| 东西 | 重启后 | 你要做什么 |
| --- | --- | --- |
| 配对关系 | 通常保留 | 一般不用重新配对 |
| Shizuku 服务 | 已停止（非 root 设备必停） | 打开 Shizuku 点一次「启动」 |
| 应用授权 | 通常保留 | 不用重新授权 |

所以「应用报 Shizuku 不可用」时，第一反应不是重新授权，而是先看 Shizuku 服务还在不在跑。

## 六、启动失败怎么办

- **一直显示「正在搜索配对服务」**：确认手机连着 WLAN（无线调试走局域网），把「无线调试」开关关掉再打开，然后重试（官方手册 FAQ 的处理口径）；
- 其他启动问题（adb 权限受限、服务随机停止等）官方手册的 FAQ 都有对应条目，对照现象去查：[shizuku.rikka.app/guide/setup](https://shizuku.rikka.app/guide/setup/)。

配好之后它解决的是「权限不够」的问题；无障碍服务被系统杀是另一回事，两者的处置互不替代，见 [无障碍权限开启教程.md](无障碍权限开启教程.md)。
