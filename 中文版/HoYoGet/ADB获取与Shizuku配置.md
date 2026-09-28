# HoYoGet 的 ADB 获取与 Shizuku 配置

> 本篇讲 ADB 获取方式的完整配置：Shizuku 从哪装、三种启动方式怎么选、各品牌手机的权限坑，以及取链接的操作顺序。
> **相关文档**：[抽卡链接获取方法.md](抽卡链接获取方法.md) · [抽卡链接怎么用与失效处理.md](抽卡链接怎么用与失效处理.md) · [常见问题与错误码.md](常见问题与错误码.md) · [账号登录与B服绑定.md](账号登录与B服绑定.md)

---

> [!IMPORTANT]
> **HoYoGet 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/d38997cf5725](https://pan.quark.cn/s/d38997cf5725)

---

## 一、这种方式适合谁、前提是什么

ADB 获取是三种方式里覆盖面最广的：原神 / 崩铁 / 绝区零、官服 / B服 / 国际服都能取。原理上它靠读取游戏日志来拿链接，所以个别限制日志输出的机型用不了。

开始前确认三件事：

- **系统版本**：安卓 11 及以上全程手机操作，连着 Wi-Fi 就行；安卓 10 及以下（含鸿蒙）首次配置需要一台电脑。
- **机型**：vivo / iQOO（OriginOS）对游戏日志输出做了限制，即使 Shizuku 正常运行也取不到链接 —— 这两个品牌只能换设备，或改用云游戏获取；玩原神的话还有一个不依赖任何工具的手动取法，见 [抽卡链接怎么用与失效处理.md](抽卡链接怎么用与失效处理.md)。
- **root 设备**：已 root 的话最简单，HoYoGet 的卡片里会出现切换模式按钮，直接以 root 权限取链接，不用走 Shizuku 启动那一套。

## 二、安装 Shizuku

Shizuku 是一个独立的授权工具，HoYoGet 借它取得读取日志的权限。下载渠道（按顺手程度排）：

| 渠道 | 地址 | 备注 |
| --- | --- | --- |
| 蓝奏云 | [https://www.lanzoul.com/s/HoYoGet-Shizuku](https://www.lanzoul.com/s/HoYoGet-Shizuku) | HoYoGet 文档推荐的上传，国内直接下 |
| IzzyOnDroid | [https://apt.izzysoft.de/fdroid/index/apk/moe.shizuku.privileged.api](https://apt.izzysoft.de/fdroid/index/apk/moe.shizuku.privileged.api) | 英文站 |
| GitHub Release | [https://github.com/RikkaApps/Shizuku/releases](https://github.com/RikkaApps/Shizuku/releases) | 英文站，有时进不去 |
| Google Play | [https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api](https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api) | 需要相应网络环境 |

版本选择注意：Shizuku 13.6.0 在部分天玑机型上跑不起来，遇到就换 13.5.4；安卓 16 则必须用 13.6.0。

## 三、启动 Shizuku 的三种方式

### 方式一：root 启动

设备已 root 的直接在 Shizuku 里点启动即可。不清楚什么是 root，就用下面两种方式。

### 方式二：无线调试启动（安卓 11+，多数人用这个）

无需电脑。配对只在首次配置时做一次（约 3~5 分钟），之后每次只需点「启动」；设备重启后要重新点一次启动（不用重新配对）。

配对步骤：

1. 开启「开发者选项」：一般进设置的系统信息页，连点 7 次「版本号」。各机型入口有差别，搜「你的机型 + 开发者选项」最稳。
2. 打开 Shizuku，点「配对」进入配对页面。
3. 给 Shizuku 通知权限 —— 待会儿要在通知里输入配对码，这个通知很重要。
4. 回到 Shizuku 配对页，读完注意事项后点「开发者选项」按钮进入系统设置。
5. 在开发者选项里启用「USB 调试」和「无线调试」。
6. 进「无线调试」页面，点「使用配对码配对设备」，记住配对码，别关这个页面。
7. 下拉通知栏，在 Shizuku 的通知里输入配对码并提交。

MIUI（小米 / POCO）看不到通知里的输入框：长按通知试试，或在系统设置的「通知管理」-「通知显示设置」里把通知样式切成「原生样式」。

配对成功后，在 Shizuku 里点「启动」，出现 `Service started, this window will be automatically closed in 3 seconds` 字样即成功。

### 方式三：连电脑启动（安卓 10 及以下 / 鸿蒙）

每次重启手机后都要重连电脑再执行一次。

1. 下载 Google 的「SDK 平台工具」并解压到任意文件夹：
   - Windows：[https://dl.google.com/android/repository/platform-tools-latest-windows.zip](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
   - Linux：[https://dl.google.com/android/repository/platform-tools-latest-linux.zip](https://dl.google.com/android/repository/platform-tools-latest-linux.zip)
   - Mac：[https://dl.google.com/android/repository/platform-tools-latest-darwin.zip](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip)
2. 在解压出的 `platform-tools` 文件夹里打开终端（Windows 按住 Shift 右键选「在此处打开 PowerShell 窗口」；Mac / Linux 开 Terminal）。输入 `adb` 回车，能看到一长串说明而不是「找不到命令」即为成功。这个窗口别关。
3. 手机上开启「开发者选项」和「USB 调试」，用数据线连上电脑，终端输入 `adb devices`；手机弹出「是否允许调试」时勾选「一律允许」并确认。
4. 在 Shizuku 里点「查看指令」（或直接用下面这条）粘进终端回车。PowerShell / Mac / Linux 下要把 `adb` 换成 `./adb`：

```bash
adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh
```

5. Shizuku 显示已启动即可。

## 四、取链接的操作顺序

1. 打开 HoYoGet，点「授权」，允许它使用 Shizuku。
2. 点「开始获取」，等它连上 Shizuku（正常约 1 秒）。
3. 去游戏里打开抽卡「历史记录」页面（原神在抽卡页左下角；崩铁是「查看详情」里的「历史记录」）。
4. 回到 HoYoGet 点「停止获取」，取到的话链接已在剪贴板。

顺序是关键：**先点「开始获取」，再去游戏里开历史记录**。提示「没有获取到链接」时，多半是顺序反了或切回来太慢，按上面顺序重来一次。链接拿到后的用法与保管注意见 [抽卡链接获取方法.md](抽卡链接获取方法.md)。

## 五、各品牌常见问题

| 现象 | 处理 |
| --- | --- |
| 一直显示「正在搜索配对服务」 | 允许 Shizuku 在后台运行；搜索配对服务要访问本地网络，不少厂商会在应用切后台后立刻断它的网 |
| 输配对码立刻失败（MIUI） | 通知样式切「原生样式」，或长按通知调出输入框 |
| 提示「adb 权限受限」（MIUI） | 开发者选项里开启「USB 调试（安全设置）」，它和「USB 调试」是两个独立开关 |
| 提示「adb 权限受限」（ColorOS，OPPO / 一加） | 开发者选项里关闭「权限监控」 |
| 提示「adb 权限受限」（Flyme，魅族） | 开发者选项里关闭「Flyme 支付保护」 |
| Shizuku 时不时停止运行 | 保证后台运行；别关 USB 调试和开发者选项；USB 使用模式改成「仅充电」（安卓 9+ 为「不进行数据传输」）；安卓 11+ 开发者选项里启用「停用 adb 授权超时功能」 |
| 同上（EMUI，华为） | 开发者选项里开启「"仅充电"模式下允许 ADB 调试」 |
| 同上（MIUI） | 别用「手机管家」的扫描功能，它会关掉开发者选项 |
| 同上（Sony） | 连 USB 后弹出的对话框别点，点了会改 USB 使用模式 |
| vivo / iQOO 怎么都取不到 | OriginOS 限制日志输出，这条路走不通，换设备或改用云游戏获取 |

按品牌处理完还是不行，去 [常见问题与错误码.md](常见问题与错误码.md) 看错误码解释，或改走 [账号登录与B服绑定.md](账号登录与B服绑定.md) 的登录方式（仅原神 / 绝区零）。
