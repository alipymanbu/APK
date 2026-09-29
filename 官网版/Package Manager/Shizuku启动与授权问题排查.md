# Shizuku 启动与授权问题排查

> 本篇讲 Package Manager 提示需要权限、但 Shizuku 侧启不来或授权不成功时的排查顺序与处理办法。
> **相关文档**：[无root能用哪些功能.md](无root能用哪些功能.md) · [批量操作与系统应用卸载.md](批量操作与系统应用卸载.md)

---

> [!IMPORTANT]
> **Package Manager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/8e49c94efec8](https://pan.quark.cn/s/8e49c94efec8)

---

## 一、先分清问题出在哪一侧

Package Manager 里的高级操作（卸载/停用系统应用、改 AppOps、清数据）自己没有特殊权限，全都借 Shizuku 的后台服务执行。所以排查顺序是：先看 Shizuku 应用自己能不能显示「运行中」——它能运行，问题才轮到 Package Manager 这一侧；它不能运行，就按下面第二节处理。

## 二、Shizuku 显示「未运行」的常见原因

| 原因 | 处理 |
| --- | --- |
| 手机刚重启过 | 免 root 启动的 Shizuku 不会开机自启，重启后要重新启动服务（root 设备可选开机自启） |
| 无线调试没开，或被系统关掉了 | 到开发者选项里重新开启；部分机型会在一段时间后自动关闭无线调试 |
| 没给 Shizuku 通知权限 | 配对过程靠通知完成，不给通知权限会卡在配对那一步 |
| 换了 Wi-Fi 网络或断网 | 无线调试依赖网络连接，换网后常需重新配对 |
| 配对码输入超时 | 配对码有时效，弹出后尽快输入；超时就重新发起配对 |

## 三、免 root 用无线调试启动（简述）

1. 设置 → 关于手机 → 连点「版本号」7 次，解锁开发者选项。
2. 开发者选项里同时开启「USB 调试」与「无线调试」——不插电脑也要开 USB 调试，这是启动流程的一部分。
3. 手机连上稳定的 Wi-Fi。
4. 打开 Shizuku 应用 → 选「通过无线调试配对」→ 按提示到开发者选项里取配对码输入。
5. 配对成功后点「启动」，状态变为「运行中」即可。

不同厂商的入口名称有差异（MIUI、ColorOS、HarmonyOS 等对开发者选项的叫法与位置不同），找不到就搜「你的机型 + 无线调试」。完整步骤以 Shizuku 官方指南为准：[shizuku.rikka.app/guide/setup/](https://shizuku.rikka.app/guide/setup/)。

## 四、授权环节的常见问题

- Package Manager 第一次通过 Shizuku 执行操作时，Shizuku 会弹「允许 Package Manager 访问」的确认框，允许即可；有的版本会问授权有效期。
- 之前点过拒绝，应用会一直被拦——进 Shizuku 应用的已授权应用列表里重新放行。
- 确认已授权、Package Manager 仍提示无权限时，把 Package Manager 从后台划掉重开一次，让它重新连 Shizuku。
- 每次重启手机后 Shizuku 都要重新启动服务，Package Manager 的授权记录一般还在，不用重复授权。

## 五、还是不行时

- 把 Shizuku 和 Package Manager 都更新到各自最新版再试。
- root 设备可以直接在 Shizuku 里选「通过 root 启动」，跳过无线调试这一整层。
- 换 ADB 路径：连电脑用 ADB 启动 Shizuku 是另一种官方支持的方式，无线调试实在配不上时用它。

权限通了之后能做什么，见 [无root能用哪些功能.md](无root能用哪些功能.md) 的对照表；下一步通常是批量清理预装应用，流程见 [批量操作与系统应用卸载.md](批量操作与系统应用卸载.md)。
