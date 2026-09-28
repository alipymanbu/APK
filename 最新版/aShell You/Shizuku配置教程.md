# aShell You 的 Shizuku 配置教程

> 讲 aShell You 为什么要配 Shizuku、Shizuku 的三种启动方式怎么选、以及在 aShell You 里怎么完成授权。
> **相关文档**：[无线调试免电脑启动方法](无线调试免电脑启动方法.md) · [下载与安装教程](下载与安装教程.md) · [常见连接问题排查](常见连接问题排查.md)

---

> [!IMPORTANT]
> **aShell You 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/60ba8c6c52c3](https://pan.quark.cn/s/60ba8c6c52c3)

---

## 一、为什么要先配 Shizuku

aShell You 本身是个普通应用，没有任何特权；你想执行的 `pm`、`wm`、`settings` 这些命令属于 shell 级（和电脑上 adb 命令同级的权限），系统不会随便把这种权限交给一个普通应用。Shizuku 解决的就是这件事：它是一个独立的权限框架应用（rikka 出品，官网 [shizuku.rikka.app](https://shizuku.rikka.app/)），启动之后会一直以 shell 级权限在后台运行，并允许其他应用借用这份权限。

所以流程是两步：先把 Shizuku 跑起来，再回到 aShell You 完成一次授权。之后你在 aShell You 里输的每条命令，都会经 Shizuku 以 shell 级权限执行。

不是所有场景都需要它：设备已 root 时可以直接用 root 方式；只对别的设备发命令（OTG 或无线）时也不需要。

## 二、三种启动方式怎么选

Shizuku 需要被「启动」一次才能工作，启动方式有三条路：

| 方式 | 前提 | 适合谁 |
| --- | --- | --- |
| 无线调试启动 | Android 11 及以上 | 没有电脑、没有 root 的绝大多数人，首选 |
| 通过 root 启动 | 设备已 root | 已 root 设备，一键最快 |
| 电脑 ADB 启动 | 一台装了 adb 的电脑 + 数据线 | Android 10 及以下（没有无线调试入口） |

三选一即可。无线调试方式的配对步骤比较长，单独写在 [无线调试免电脑启动方法](无线调试免电脑启动方法.md) 里；这里说另外两条：

- **root 启动**：打开 Shizuku，点「通过 root 启动」，在 root 管理器里放行即可；
- **电脑 ADB 启动**：手机连电脑开 USB 调试，在电脑上执行 Shizuku 界面给出的那条 `adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh`（以 Shizuku 界面与 [官方文档](https://shizuku.rikka.app/guide/setup.html) 当时的说明为准）。

## 三、装 Shizuku 并启动

1. 安装 Shizuku 本体：Play 商店或官方仓库 [RikkaApps/Shizuku](https://github.com/RikkaApps/Shizuku)（认准包名 `moe.shizuku.privileged.api`，别装到仿冒应用）；
2. 按第二节的表选一种方式启动；
3. 启动成功的标志是 Shizuku 主界面显示服务正在运行。

启动方式只决定服务怎么被拉起，对 aShell You 的用法没有影响。

## 四、在 aShell You 里完成授权

1. 确认 Shizuku 主界面显示「运行中」；
2. 打开 aShell You，选择本地（Shizuku）连接方式；
3. 首次连接会弹出 Shizuku 的授权对话框，点允许；
4. 回到命令输入界面，随便执行一条无害命令（比如 `pm list packages`）验证能出输出。

如果第 2 步提示检测不到 Shizuku，先看 [常见连接问题排查](常见连接问题排查.md) 的检测不到一节。

## 五、重启手机之后

Shizuku 的服务会随系统重启而停止，这是正常现象，不是出了故障：

- **配对记录不会丢**：无线调试方式只需配对一次，重启后重复「打开 Shizuku → 通过无线调试启动」即可，不用再输配对码；
- **授权记录也不会丢**：aShell You 的授权一次就永久生效，重启后不用重新允许。

也就是说，重启后你只需要重新执行一次「启动」动作。无线调试的具体步骤见 [无线调试免电脑启动方法](无线调试免电脑启动方法.md)。

## 六、root 用户的另一条路

已 root 的设备有两条现成的路：一是照常装 Shizuku 并用「通过 root 启动」；二是直接在 aShell You 里选 root 连接方式，跳过 Shizuku。两者执行的命令权限同级，选顺手的即可。
