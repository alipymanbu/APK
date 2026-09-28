# Extendroid 的 Shizuku 激活与授权设置

> Extendroid 依赖 Shizuku 才能弹出可自由缩放的应用窗口。本篇讲 Shizuku 是什么、怎么启动、怎么授权给 Extendroid，以及重启手机后要做什么。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [多窗口使用方法与手势操作.md](多窗口使用方法与手势操作.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **Extendroid 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a5ac7f82b356](https://pan.quark.cn/s/a5ac7f82b356)

---

Extendroid 要把任意应用装进可缩放的窗口，需要一层系统级权限，这层权限由 Shizuku 提供 —— 官方把「Shizuku 可用」列为硬性要求，开不出窗口时先查它。手机不需要 Root。还没装 Extendroid 的话，先看 [下载与安装教程.md](下载与安装教程.md)。

## 一、Shizuku 是什么、为什么需要它

Shizuku 是一个给其他应用「借」系统权限的工具，官方主页：[shizuku.rikka.app](https://shizuku.rikka.app)。它自己先拿到一层系统权限，再把权限转授给 Extendroid 这类应用。Extendroid 运行期间，Shizuku 必须处于运行状态。

## 二、启动 Shizuku 的常用方式

| 方式 | 适合谁 | 要点 |
| --- | --- | --- |
| 无线调试 | Android 11 及以上 | 配对只需成功一次；日常启动不用连电脑 |
| 电脑 adb | Android 10 设备 / 无线调试失败 | 手机连电脑，按 Shizuku 官方文档执行命令 |
| Root | 已 Root 的设备 | 授权后可设置开机自启 |

具体命令与界面以 Shizuku 官方文档（[shizuku.rikka.app](https://shizuku.rikka.app)）为准，不同安卓版本的入口叫法略有差异。

## 三、把权限授给 Extendroid

1. 打开 Shizuku，确认主界面显示服务「运行中」；
2. 打开 Extendroid，按首页设置项的引导完成授权（也可以在 Shizuku 的「使用 Shizuku 的应用」列表里手动允许 Extendroid）；
3. 回到 Extendroid 首页，确认设置项已变为完成状态。

## 四、重启手机之后要做什么

Shizuku 的服务在手机重启后会停止，这时 Extendroid 又开不出窗口了 —— 不是 Extendroid 坏了，重新启动一次 Shizuku 就行（配对不用重做）。用 Root 方式启动的可以设置开机自启，省掉这一步。配对不上、启动报错或服务频繁停止时，按 [Shizuku配对失败与启动报错处理.md](Shizuku配对失败与启动报错处理.md) 处理；其他「开不出窗口」的症状去 [常见问题排查.md](常见问题排查.md) 对清单。
