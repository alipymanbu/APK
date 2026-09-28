# Shizuku 授权与文件处理器设置

> 本篇解决小米、华为、荣耀等机型在 Android 11+ 上「字体应用失败」的问题：给 zFont 3 配一个文件处理器，推荐用 Shizuku。
> **相关文档**：[免root机型兼容清单.md](免root机型兼容清单.md) · [换字体步骤详解.md](换字体步骤详解.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **zFont 3 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f5045645f27d](https://pan.quark.cn/s/f5045645f27d)

---

## 一、为什么需要文件处理器

小米（MIUI、HyperOS）、华为、荣耀、Tecno、Infinix 这几家的主题系统把字体文件存在 `Android/data` 目录下。从 Android 11 开始，系统限制普通应用直接读写这个目录，所以 zFont 3 把字体放进去时会失败。文件处理器就是替 zFont 3 拿到这个目录访问权限的一层代理。

三星、LG、OPPO、vivo 等走主题商店接口的机型不涉及此目录，无需配置这一步。

## 二、文件处理器方式怎么选

官方给出的方式与适用范围如下（截至检索时）：

| 方式 | 配置难度 | 适用范围 | 是否推荐 |
| --- | --- | --- | --- |
| Legacy（旧版直连） | 简单 | Android 10 及以下 | 系统老就不用配，直接用 |
| SAF（存储访问框架） | 简单 | Android 11–12 可靠，13+ 基本失效 | 系统版本低时可作备选 |
| Shizuku | 中等 | 全部设备、全部版本 | 首选 |
| zFile | 简单 | 因机型而异 | 不推荐，成功率不稳定 |
| Root | 高 | 全部设备 | 已 root 用户可用，但仍建议用 Shizuku |

判断规则：Android 10 及以下什么都不用配；Android 11–12 可以先试 SAF（简单），不行再上 Shizuku；Android 13 及以上直接用 Shizuku。

### 附：SAF 方式的设置与排错

Android 11–12 上 SAF 不用装任何东西，设置路径：zFont 3 → Settings → File Handler → 选 **Storage Access Framework** → 点 Get Started → 系统跳转到主题目录时点底部「Use this folder（使用此文件夹）」。File Handler 状态显示为 Storage Access Framework 即配置完成。

| 现象 | 处理方法 |
| --- | --- |
| 一进来就提示 SAF Unsupported | 点 Ignore 忽略提示，勾选 **Use bypass** 后再点 Get Started 试一次；仍不行就换 Shizuku |
| 「使用此文件夹」按钮点不了 | 切换 Use bypass 的勾选状态（开了就关、关了就开）再试 |
| 系统更新后 SAF 突然失效 | 回到 File Handler 重新授权一次；反复失效就换 Shizuku，一劳永逸 |
| Android 13+ 上怎么试都不行 | 预期行为：多数机型在 Android 13 起封锁了 SAF 访问，直接改用 Shizuku |

## 三、Shizuku 配置步骤

Shizuku 是一个独立的授权工具应用，你需要先装它（Google Play 或 GitHub 的 RikkaApps/Shizuku 发布页都可以下到），然后把手机的开发者选项打开。

### 1. 打开开发者选项

- 华为、荣耀及多数机型：设置 → 关于手机 → 连续点「版本号」7 次。
- 小米（MIUI）：设置 → 我的设备 → 连续点「MIUI 版本」7 次。
- 小米（HyperOS）：设置 → 我的设备 → 连续点「OS 版本」7 次。
- Tecno、Infinix：设置 → 我的手机 → 连续点「版本号」7 次。

### 2. 用无线调试配对（Android 11+，推荐，无需电脑）

1. 打开 Shizuku 应用，允许它发的所有权限请求，点「Pairing（配对）」，并开启通知权限。
2. 回到 Shizuku 提示的开发者选项页，打开「USB 调试」，再找到「无线调试」并打开。
3. 点「无线调试」里的「使用配对码配对设备」，通知栏会弹出 Shizuku 的配对输入框，把屏幕上的配对码填进去，点配对。
4. 回到 Shizuku，点「Start（启动）」，看到「Shizuku is running」即成功。

没看到配对弹窗时，下拉通知栏找 Shizuku 的「Enter pairing code」通知点进去填码。

### 3. 用电脑 ADB 启动（Android 10 及以下或无线配对不通时）

在电脑上装 ADB 工具，手机连电脑开启 USB 调试后执行 `adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh`（具体命令以 Shizuku 官方指南为准）。没有电脑时，可以用另一台安卓手机装 Bugjaeger 或 ADB OTG，通过 OTG 线充当 ADB 主机。

### 4. 在 zFont 3 里启用

Shizuku 跑起来之后：

1. 打开 zFont 3。
2. 进 Settings → File Handler（文件处理器）。
3. 选 **Shizuku**，在弹出的授权框里允许。
4. 完成后回到字体页正常应用字体即可。

Shizuku 启动后会一直保持运行，直到手机重启；重启后需要重新点一次「Start」，配对不用重做。

## 四、Shizuku 排错

| 现象 | 处理方法 |
| --- | --- |
| 配对成功但点 Start 没反应 | 开发者选项里把「无线调试」关掉再打开，回 Shizuku 重新点 Start |
| 提示 Service Start Failed | 打开 Shizuku 的「应用管理」，把 zFont 开关关掉，重开 zFont 再试 |
| 手机重启后失效 | 正常现象，重新点一次 Start 即可，无需重新配对 |
| 反复失败 | 换用社区维护的 Shizuku 分支版本（官方文档有提及），或改走电脑 ADB 启动 |

配好文件处理器后，回到[换字体步骤详解](换字体步骤详解.md)继续应用字体；如果还失败，按该篇第五节逐项排查。
