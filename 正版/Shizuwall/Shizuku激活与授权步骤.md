# Shizuku 激活与 ShizuWall 授权步骤

> ShizuWall 依赖 Shizuku 提供的授权通道才能执行系统级断网；本篇讲三种激活方式、授权过程与失败排查。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [Shizuku启动失败排查.md](Shizuku启动失败排查.md) · [重启后规则失效怎么办.md](重启后规则失效怎么办.md) · [常见问题解答.md](常见问题解答.md)

---

> [!IMPORTANT]
> **ShizuWall 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/1c97432c48fb](https://pan.quark.cn/s/1c97432c48fb)

---

## 一、为什么装完还不能直接用

ShizuWall 自己不带系统权限，它通过 Shizuku 拿到一段系统级授权，再去调用安卓原生的网络管理接口切断应用网络。所以顺序固定是：

1. 先激活 Shizuku（本篇主体）；
2. 打开 ShizuWall，接受 Shizuku 授权弹窗；
3. 之后防火墙才能正常启用。

Shizuku 是安卓社区使用多年的授权框架，官网 [shizuku.rikka.app](https://shizuku.rikka.app/)，源码同样公开。如果 ShizuWall 本体还没装好，先看 [下载与安装教程.md](下载与安装教程.md)。

## 二、方式一：无线调试激活（Android 11+，不用电脑）

Android 11 及以上设备的常规路子，全程在手机上完成：

1. 安装 Shizuku 并打开（官网或其 GitHub 发布页可下载）。
2. 进入「无线调试」配对流程，按提示到系统「开发者选项」里打开「无线调试」开关。
3. 点「配对」→ 输入屏幕上显示的配对码 → 配对成功后回到 Shizuku 点「启动」。
4. Shizuku 状态显示「运行中」即完成。

配对码随页面刷新而变化，超时就重新生成一个再输。开发者选项没解锁的设备，先到「设置 → 关于手机」里连点「版本号」七次解锁开发者选项；Shizuku 官方步骤还要求把「USB 调试」一并打开，哪怕全程用不到数据线。

两个官方口径值得记住：**配对只需一次**，之后重启手机也不用重新配对；如果点「启动」没反应，官方建议先把「无线调试」禁用再启用一次。配对反复失败的逐机型解法见 [Shizuku启动失败排查.md](Shizuku启动失败排查.md)。

## 三、方式二：电脑 ADB 激活

设备低于 Android 11、或无线调试反复失败时用这条路：

1. 电脑下载 Google 官方的「SDK 平台工具」（platform-tools，内含 ADB）并解压：[Windows](https://dl.google.com/android/repository/platform-tools-latest-windows.zip) / [Linux](https://dl.google.com/android/repository/platform-tools-latest-linux.zip) / [Mac](https://dl.google.com/android/repository/platform-tools-latest-darwin.zip)。
2. 手机打开「开发者选项 → USB 调试」，数据线连电脑，在终端里输入 `adb devices`；手机弹出「是否允许调试」时勾选「总是允许」再确认。
3. 在 Shizuku 应用里查看它展示的启动命令并执行，一般为：

   ```bash
   adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh
   ```

   以应用内当时显示的命令为准；在 PowerShell 或 Mac/Linux 终端里，官方提示要把 `adb` 写成 `./adb`。
4. Shizuku 显示「运行中」即可拔线使用。

官方对这条路的适用口径是「未 root 且运行 Android 10 及以下」的设备，Android 11+ 优先走无线调试；手机重启后同样要重跑一次启动命令。

## 四、方式三：已 Root 的设备

设备本身有 root 的话，打开 Shizuku 直接点「启动」即可，不涉及配对与电脑。

## 五、在 ShizuWall 里完成授权

1. 确认 Shizuku 状态为「运行中」。
2. 打开 ShizuWall，首次进入会申请 Shizuku 授权，点允许；部分系统会再弹一次确认。
3. 授权成功后即可看到应用列表并启用防火墙。

官方介绍里还提到 ShizuWall 内置「本地 ADB」激活方式，作为不装 Shizuku 应用时的替代；入口与可用性以你设备上应用内的实际选项为准。

## 六、激活失败的排查

| 现象 | 处理 |
| --- | --- |
| 换了数据线 ADB 还是连不上 | 确认手机上已允许「USB 调试」授权弹窗（勾「总是允许」）；仅充电的数据线不传数据，换一根能传数据的 |
| 授权弹窗一闪而过或失败 | 回到 Shizuku 确认「运行中」，重开 ShizuWall 再试；仍失败就重启手机从头来一遍 |
| 一直「正在搜索配对服务」、输配对码就失败、adb 权限受限、随机停止 | 这些按品牌有专门开关，逐现象的官方解法见 [Shizuku启动失败排查.md](Shizuku启动失败排查.md) |
| 重启手机后又要重新激活 | 授权通道重启后会断，属正常机制；减少手动操作的办法见 [重启后规则失效怎么办.md](重启后规则失效怎么办.md) |

更多使用层面的疑问汇总在 [常见问题解答.md](常见问题解答.md)。
