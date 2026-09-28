# Shizuku 授权与激活方法

> InstallerX 在没有 ROOT 的手机上主要靠 Shizuku 拿安装权限，本篇从激活 Shizuku 到给 InstallerX 授权一步步走完。
> **相关文档**：[锁定默认安装器方法.md](锁定默认安装器方法.md) · [下载与安装教程.md](下载与安装教程.md) · [安装失败排查方法.md](安装失败排查方法.md) · [Dhizuku激活与授权方法.md](Dhizuku激活与授权方法.md)

---

> [!IMPORTANT]
> **InstallerX 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/63730afdbf40](https://pan.quark.cn/s/63730afdbf40)

---

## 一、三种授权器怎么选

InstallerX 调安装接口需要特权，授权器就是给它供权限的通道：

| 授权器 | 前提 | 特点 |
| --- | --- | --- |
| ROOT 超级用户 | 手机已 ROOT | 最直接，授权一次长期有效 |
| Shizuku | 无需 ROOT，Android 11+ 可用无线调试激活 | 无 ROOT 用户的主流选择，功能完整 |
| Dhizuku | 无需 ROOT，走设备管理员机制 | 激活门槛高，权限相对弱，部分高级操作做不到 |

你的手机已 ROOT 就直接选 ROOT 超级用户；没 ROOT，装一个 [Shizuku](https://github.com/RikkaApps/Shizuku)（开源免费）再回来授权是省事路线。Dhizuku 适合既没 ROOT 又用不了 Shizuku 的少数场景。

## 二、激活 Shizuku（Android 11 及以上：无线调试）

前提：手机连着 Wi-Fi，开发者选项已打开。

1. 打开手机的**设置 → 开发者选项**，开启**无线调试**；
2. 打开 Shizuku 应用，点**通过无线调试启动**，再进**配对**页面；
3. 回到系统的无线调试界面，选**使用配对码配对设备**，屏幕会显示一组 6 位配对码；
4. 下拉通知栏，Shizuku 会发来一条"已找到配对服务，请输入配对码"的通知，把 6 位码填进去；
5. 回到 Shizuku 主界面，看到"Shizuku 正在运行"即激活成功。

激活成功后别急着关 Shizuku——它要在后台保持运行，InstallerX 才能随时借到权限。

配对或启动卡住的两种官方口径（出自 Shizuku 用户手册的 FAQ）：

- **一直停在"正在搜索配对服务"**：多半是无线调试没有真正处于开启状态，把系统的无线调试关掉再重新打开，回 Shizuku 重试；
- **输完配对码立刻失败**：配对码有时效，超时作废；重新生成一组后第一时间填进通知，别中途停留；
- **点启动没反应**：官方给的排查就是先关闭再开启无线调试，然后再点启动。

## 三、Android 11 以下：用电脑 ADB 激活

老系统没有无线调试配对入口，需要电脑连一次：

1. 电脑安装 ADB 工具（Android platform-tools），手机开 USB 调试并连接；
2. 在电脑上执行 `adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/start.sh`（以 Shizuku 应用内显示的命令为准，不同版本可能略有差异）；
3. 手机上 Shizuku 显示正在运行即可，之后拔线不影响（重启手机后需要重新执行一次）。

## 四、把权限授给 InstallerX

1. 打开 Shizuku，进入**使用 Shizuku 的应用**（有的版本叫"已授权应用"）；
2. 找到 InstallerX，打开它的授权开关；
3. 如果列表里没有 InstallerX，先在 InstallerX 里随便发起一次授权请求（比如进设置点授权器），它就会出现在候选列表里。

## 五、回到 InstallerX 完成设置

1. 打开 InstallerX，进**设置**，把**授权器**选为 Shizuku；
2. 点**锁定为默认安装器**（这步必须在授权器选好之后做，详见 [锁定默认安装器方法.md](锁定默认安装器方法.md)）；
3. 找一个 APK 试装，弹出 InstallerX 的安装对话框即成功。

1.6 版本起 InstallerX 加了授权复用：短时间内连续装多个包不会反复弹授权申请，批量安装时能省不少点击。

## 六、重启手机后 Shizuku 失效

无线调试方式激活的 Shizuku 在重启后会停，这是机制决定的，不是故障：

- 手机上重新走一遍"通过无线调试启动"即可（一般不用重新配对）；
- 嫌麻烦可以把 Shizuku 加进电池白名单减少被杀（官方 FAQ 里"Shizuku 随机停止"指向的就是后台限制），或者改用重启后依然有效的 Dhizuku，见 [Dhizuku激活与授权方法.md](Dhizuku激活与授权方法.md)。

授权相关的其他报错（比如点授权没反应、授权器打开失败）见 [安装失败排查方法.md](安装失败排查方法.md)。
