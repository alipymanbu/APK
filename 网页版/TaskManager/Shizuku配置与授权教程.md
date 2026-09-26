# TaskManager 需要的 Shizuku 配置与授权教程

> Shizuku 是 TaskManager 能干活的前提：本篇带你把 Shizuku 装好、启动起来、再授权给 TaskManager。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [结束进程与后台应用的方法.md](结束进程与后台应用的方法.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **TaskManager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/80f324d3d40a](https://pan.quark.cn/s/80f324d3d40a)

---

## 一、为什么 TaskManager 非要 Shizuku 不可

安卓的沙箱机制下，普通 App 只能看自己、看不到别的应用在占多少 CPU、也没权限把别的应用结束掉。Shizuku（开源，Rikka 开发）的做法是用 adb 或 root 起一个常驻服务，让被授权的 App 借用这个服务的权限去调系统接口——效果接近 root，但你不用给手机解锁 root。

TaskManager 官方说明写得很直白：**它需要 Shizuku 或 root 才能工作**。所以流程永远是三步：装 Shizuku → 启动 Shizuku → 授权给 TaskManager。

## 二、装 Shizuku

Google Play 搜 `moe.shizuku.privileged.api`，或到 GitHub 的 `RikkaApps/Shizuku` 仓库 Releases 页下载，装法与普通 App 相同。装完先放着，下一步启动它。

## 三、启动 Shizuku 的三种方式

按你的手机情况选一种，**优先级建议：有 root 用 root，没 root 且系统是 Android 11 以上用无线调试，再不行才连电脑**。

### 方式一：通过 root 启动

手机已 root 的话最简单：打开 Shizuku 点「启动」，授权弹窗里同意即可。root 方式启动的服务**重启手机后依然有效**（是否开机自启取决于你的 root 管理器设置）。

### 方式二：通过无线调试启动（Android 11 及以上，不需要电脑）

1. 手机「设置 → 关于手机」里连点版本号 7 次，打开开发者选项。
2. 进「系统 → 开发者选项」，打开「USB 调试」，再进入「无线调试」把它也打开。
3. 打开 Shizuku，点「配对」进入配对流程。
4. 回到系统的「无线调试」页面，点「使用配对码配对设备」，会显示 6 位配对码和端口号。
5. 在 Shizuku 的通知栏输入框里填入配对码，提示配对成功。**配对只需做一次。**
6. 回到 Shizuku 点「启动」，看到「Shizuku 正在运行」即成功。

由于系统限制，**这种方式每次重启手机后都要重新点一次「启动」**（配对不用重做）。启动不起来时，把无线调试关掉再开一次通常就能解决。

### 方式三：通过连接电脑启动（Android 10 及以下）

适合没 root 的老系统：手机连电脑，电脑装好 adb，执行 `adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/files/start.sh`（命令以 Shizuku 界面当时显示的为准）把服务拉起来。同样地，**重启后要重跑一次**。

## 四、授权给 TaskManager

1. 确认 Shizuku 主界面显示「正在运行」。
2. 打开 TaskManager，首次进入会弹 Shizuku 的授权请求，点「**始终允许**」。
3. 回到 Shizuku 的「已授权应用」列表确认 TaskManager 已勾上；如果列表显示的数量不对，把 Shizuku 从后台杀掉重进一次再看。

授权完成后，TaskManager 里的 CPU、内存、进程列表才能刷出数据。结束进程、调整优先级这类操作对权限要求更高（详见 [结束进程与后台应用的方法.md](结束进程与后台应用的方法.md)）。

## 五、日常使用的两个习惯

- **重启手机后先开 Shizuku，再开 TaskManager**：非 root 启动的服务不跨重启，忘了开就会出现「之前好好的，现在又没数据了」。
- **别卸载 Shizuku 或关掉开发者选项**：无线调试被关掉，Shizuku 就启动不了，TaskManager 跟着瘫。

配对卡住、授权不生效、重启后失效等问题，集中放在 [常见问题与故障排查.md](常见问题与故障排查.md)。
