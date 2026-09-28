# Swift Backup 免 Root 使用与 Shizuku 配置

> 本篇讲清楚无 Root 设备上 Swift Backup 哪些能做哪些不能做，以及 Shizuku 的配置步骤与重启后的注意事项。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [能备份什么与云盘选择.md](能备份什么与云盘选择.md) · [换机还原与数据迁移.md](换机还原与数据迁移.md)

---

> [!IMPORTANT]
> **Swift Backup 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/5c84c0a7a555](https://pan.quark.cn/s/5c84c0a7a555)

---

## 一、免 Root 的能力边界

无 Root 设备上能备份/还原的内容：应用 APK、短信、通话记录、壁纸；外部数据（Android/data/）在 Android 10 及以下也能直接处理，Android 11+ 需要 Shizuku 或 Root。

做不到的：

- **应用私有数据**：/data/data/ 目录任何应用都无法在无 Root 时访问，Shizuku/ADB 模式也够不到；
- **特殊应用数据**（权限、电池优化设置、SSAID 等）：跟随上一条，一起不可用；
- **批量还原应用**：官方注明只有 Root 或 Shizuku 服务运行时才支持；两者都没有时只能逐个手动还原。

还原侧各部件的要求与这张边界一一对应，见 [换机还原与数据迁移.md](换机还原与数据迁移.md)。

## 二、为什么需要 Shizuku

Shizuku 让应用以 ADB 级权限执行操作，在 Swift Backup 里解决两件事：

1. Android 11+ 上访问 Android/data/ 里的外部数据（系统收严了这块目录的访问）；
2. 应用的批量还原操作。

它替代不了 Root——私有数据那一层（/data/data/）它摸不到，这是 Android 的权限设计决定的，不是配置问题。

## 三、配置步骤

1. 手机上安装 Shizuku（Google Play 或其 GitHub 发布页都有）；
2. 打开系统开发者选项里的「无线调试」（Android 11+ 的推荐方式），按 Shizuku 应用内的指引完成配对并启动服务；
3. 回到 Swift Backup，弹出 Shizuku 授权框时选「始终允许」，避免每次操作都要重新批准；
4. 重启手机后 Shizuku 服务会停，需要重新拉起服务才能继续批量操作。

具体配对界面随 Android 版本有差异，以 Shizuku 官方文档为准。

## 四、Root 设备的差异

Root 设备首次授权走 Magisk 请求，选 Grant 即可。相比 Shizuku，Root 多出三块能力：应用私有数据（/data/data/）的备份与还原、特殊应用数据（含 Magisk Hide 状态、SSAID 等）、WiFi 配置在旧版本 Android 上的覆盖。

## 五、版本差异速查

| 你的情况 | 能做到的事 |
| --- | --- |
| 无 Root、无 Shizuku、Android 11+ | APK / 短信 / 通话记录 / 壁纸可备；外部数据不可；只能逐个还原 |
| 无 Root、无 Shizuku、Android 10 及以下 | 同上，但外部数据可以直接备份还原 |
| 无 Root + Shizuku、Android 11+ | 外部数据可备；应用可批量还原；私有数据仍不可 |
| Root | 全部能力，含私有数据与特殊应用数据 |
| 无 Root + Shizuku（5.1.0、Android 11+） | WiFi 配置也可备份还原 |

另一个已知限制：Android 10 上 WiFi 备份无法批量还原，官方 issues 页对此有专门说明；个案清单见 [读不到备份与还原失败排查.md](读不到备份与还原失败排查.md)。
