# RootlessJamesDSP 的 Shizuku 激活与权限配置

> 本篇讲为什么第一次启动要激活、Shizuku 的两种启动方式（无线调试配对 / 连电脑 ADB）怎么操作、重启后失效怎么处理。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md) · [音效功能与AutoEQ调音.md](音效功能与AutoEQ调音.md)

---

> [!IMPORTANT]
> **RootlessJamesDSP 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/2e9d16ccec1e](https://pan.quark.cn/s/2e9d16ccec1e)

---

## 一、为什么第一次启动要多一道激活

RootlessJamesDSP 要加工的是**其他应用**播放的声音，这种「全局接管」靠普通应用权限做不到，需要一次 ADB 级别的授权。免 root 的方案是借 Shizuku 之手：Shizuku 先拿到 ADB 权限，再把其中 RootlessJamesDSP 需要的部分转授给它。**授权只需要做一次**；但 Shizuku 服务在每次重启手机后都要重新启动一次，这是免 root 模式下的系统限制，不是配置丢了。

## 二、三种方式怎么选

| 方式 | 适合谁 | 需要什么 |
| --- | --- | --- |
| Shizuku（无线调试配对） | Android 11 及以上 | 只用手机，不用电脑 |
| Shizuku（连电脑启动） | Android 10，或无线调试一直配不上 | 电脑装 adb + 数据线 |
| 直接用 Root 授权 | 手机已 root | 无 |

激活方式在 RootlessJamesDSP 第一次启动的向导里选择，拿不准就按上表对号入座。

## 三、方式一：无线调试配对（Android 11+，全程手机上完成）

1. 手机上装好 Shizuku（Google Play 搜 Shizuku，或官网 [shizuku.rikka.app](https://shizuku.rikka.app)）；RootlessJamesDSP 还没装的话先看 [下载与安装教程.md](下载与安装教程.md)。
2. 打开系统设置 → 开发者选项，同时开启「USB 调试」和「无线调试」。
3. 打开 Shizuku，选「通过无线调试启动」→ 开始配对。
4. 回到系统的「无线调试」页面，点「使用配对码配对设备」，会弹出一组配对码和端口。
5. 在 Shizuku 的通知里填入这组配对码，确认配对。
6. 配对成功后回 Shizuku 点「启动」。
7. 打开 RootlessJamesDSP，点「授予访问权限」，在 Shizuku 弹出的授权框里允许 —— 界面提示设置完毕就完成了。

配对只做一次；以后每次重启手机，只需要重新执行第 6 步（Shizuku 里点一次启动）。

## 四、方式二：连电脑用 ADB 启动（Android 10 适用）

1. 电脑上下载 Google 官方的 platform-tools（内含 adb），解压备用。
2. 手机开发者选项里打开「USB 调试」，数据线连电脑，USB 用途选「传输文件」。
3. 在电脑终端里执行：

```bash
adb devices
```

   第一次连接时手机上会弹「是否允许 USB 调试」，勾选一律允许，能看到设备列表即连接成功。
4. 启动 Shizuku 服务：

```bash
adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/files/start.sh
```

5. 之后与方式一的第 7 步相同：在 RootlessJamesDSP 里点「授予访问权限」并允许。

启动命令以 Shizuku 应用内展示的为准，不同版本路径可能微调；卡在配对或权限环节，直接查 [Shizuku 官方手册](https://shizuku.rikka.app/zh-hans/guide/setup/) 的「常见问题」章节。

## 五、激活成功的判断

向导界面提示设置完毕、进入主界面不再出现权限提示，就是激活成功了。立刻验证一遍：开总开关 → 放首歌 → 拉均衡器听变化，有变化就全部就绪。

## 六、每次重启后要重新启动 Shizuku

免 root 方式下，Shizuku 的服务随系统重启而停止，RootlessJamesDSP 这边的授权记录不会丢，你只要：

1. 打开 Shizuku，点一下「启动」（无线调试方式），或连电脑再执行一次启动命令；
2. 如果 RootlessJamesDSP 提示权限失效，重新点一次「授予访问权限」。

给 Shizuku 关掉电池优化、允许自启动，能减少它被系统提前杀掉的情况 —— 但重启后重新点一次启动这一步省不掉。

## 七、激活这步的常见卡点

搜不到配对服务、填码立刻失败、点启动没反应、跑着跑着自己停 —— 这些都在 [Shizuku配对失败与服务停止处理.md](Shizuku配对失败与服务停止处理.md) 里按报错现象逐条列了解法；激活完成之后的没声音、后台被杀等问题，去 [常见问题与故障排查.md](常见问题与故障排查.md)。
