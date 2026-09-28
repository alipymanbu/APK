# Dhizuku 激活与授权方法

> Dhizuku 是 InstallerX 作者的另一件作品，走设备管理员路线给应用供权限；本篇讲什么情况下用它、激活前必须做什么准备，以及两种激活方式。
> **相关文档**：[Shizuku授权与激活方法.md](Shizuku授权与激活方法.md) · [锁定默认安装器方法.md](锁定默认安装器方法.md) · [安装失败排查方法.md](安装失败排查方法.md)

---

> [!IMPORTANT]
> **InstallerX 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/63730afdbf40](https://pan.quark.cn/s/63730afdbf40)

---

## 一、什么时候轮到 Dhizuku

Dhizuku 与 InstallerX 出自同一作者（Rosan / iamr0s），思路是把"设备所有者（Device Owner）"权限共享给支持它的应用。三种授权器里它排第三顺位：

- 有 ROOT → 直接选 ROOT 超级用户，不用看这篇；
- 没 ROOT → 大多数人走 Shizuku（见 [Shizuku授权与激活方法.md](Shizuku授权与激活方法.md)），门槛低得多；
- 没 ROOT、又不想每次重启手机都重新启动 Shizuku → Dhizuku 是备选，它激活一次长期有效。

Dhizuku 官方标注支持 Android 8.0～17（[iamr0s/Dhizuku](https://github.com/iamr0s/Dhizuku)）。

## 二、激活前提：设备上不能有账号

官方文档把这条写成醒目警告：激活 Dhizuku 前设备上**不能登录任何账号**（Google 账号、厂商账号都不行），也不能有多用户或手机分身；已登录的账号要先移除，否则激活会失败，而且官方明确不为"带账号激活"提供支持。

动手前先备份资料。如果设备上账号多、重新登录成本高，Dhizuku 的性价比就要重新掂量——不少人在这一步选择回头用 Shizuku，这是正常取舍，不算失败。

## 三、两种激活方式

### 方式一：经 Shizuku 激活（先试这条）

1. 先按 [Shizuku授权与激活方法.md](Shizuku授权与激活方法.md) 把 Shizuku 激活并保持运行；
2. 从官方 GitHub 安装 Dhizuku，打开应用，选择通过 Shizuku 激活；
3. Dhizuku 界面显示已获得设备管理员身份即成功。

### 方式二：电脑 ADB 命令激活

手机开 USB 调试连电脑，在终端执行：

```bash
adb shell dpm set-device-owner com.rosan.dhizuku/.server.DhizukuDAReceiver
```

执行后弹出一长串报错、Dhizuku 没变成已激活状态，基本都是设备上还残留账号或多余用户没清干净——回到第二节处理完再试。

官方的详细激活指引在 GitHub Discussions 的[置顶帖](https://github.com/iamr0s/Dhizuku/discussions/19)，不同机型的差异以那篇为准。

## 四、在 InstallerX 里启用

激活 Dhizuku 后，打开 InstallerX 进**设置**，把**授权器**选为 Dhizuku，再按 [锁定默认安装器方法.md](锁定默认安装器方法.md) 完成锁定。之后重启手机 InstallerX 依然可用——不需要像 Shizuku 那样每次重启都重新启动一次，这是它最省心的地方。

## 五、三种授权器怎么取舍

| 对比项 | ROOT | Shizuku | Dhizuku |
| --- | --- | --- | --- |
| 前提 | 已 ROOT | 无需 ROOT，激活门槛低 | 无需 ROOT，需清空账号，门槛最高 |
| 重启后 | 一直有效 | 需重新启动 Shizuku | 一直有效 |
| 权限强度 | 最强 | Shell 级，装包功能完整 | 设备管理员级，个别安装相关的高级操作做不到 |

装包这个用途上，Shizuku 与 Dhizuku 的覆盖面接近；Dhizuku 激活反复失败、或个别安装操作报权限不足时，切回 Shizuku 是最快的解法。授权环节的报错见 [安装失败排查方法.md](安装失败排查方法.md)。
