# Shizuku免Root权限开启教程

> 这篇讲不 root 怎么用 Shizuku 给 SD Maid SE 提权，以及它和 root 的差别。
> **相关文档**：[首次启动权限设置.md](首次启动权限设置.md) · [卸载残留与缓存清理教程.md](卸载残留与缓存清理教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **SD Maid SE 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/977a3f9450f1](https://pan.quark.cn/s/977a3f9450f1)

---

## 一、为什么需要 Shizuku

不给任何提权时，SD Maid SE 只能在系统允许的公共目录里干活：应用私有目录（`/Android/data/`、`/data/data/` 里的缓存）它扫得到但删不动，这部分结果只能引导你手动处理。两条提权路线：

- **Root**：权限最深，能碰系统分区，但要解锁/bootloader、动系统，部分银行与支付应用会拒跑；
- **Shizuku**：开源的中间层，用 ADB 授权让应用获得接近 root 的系统 API 调用能力，不改系统、不触发 root 检测，SD Maid SE 官方直接支持。

多数人只需要 Shizuku —— 它能覆盖 root 能力的大部分清理场景。

## 二、启动 Shizuku（三选一）

**方式 A：无线调试（Android 11+，推荐，无需电脑）**

1. 手机上装好 Shizuku（Google Play 或 `https://github.com/RikkaApps/Shizuku/releases`）；
2. 开发者选项：系统设置 → 关于手机 → 连点"版本号"7 次 → 返回设置出现**开发者选项**；
3. 开发者选项里启用 **USB 调试**，再进入**无线调试**并启用；
4. 打开 Shizuku → 选"通过无线调试启动" → 点**配对**，在系统"无线调试"页选"使用配对码配对设备"，把配对码填进 Shizuku 的通知；
5. 回到 Shizuku 点**启动**。看到"正在运行"即成功。

**方式 B：连接电脑（Android 10 及以下）**
电脑装好 platform-tools，手机开 USB 调试并连电脑，在 `adb` 目录执行：

```bash
adb shell sh /sdcard/Android/data/moe.shizuku.privileged.api/files/start.sh
```

**方式 C：root 启动**
已 root 的手机在 Shizuku 里直接点"使用 root 启动"，且重启后自动恢复。

**关键限制**：方式 A 和 B 都是内存态授权，**手机重启后要重新启动一次 Shizuku**（配对不用重做，启动那步要重来）；方式 C 才是永久的。

## 三、在 SD Maid SE 里授权

1. 保持 Shizuku 处于"正在运行"；
2. 打开 SD Maid SE，首次使用提权功能时会弹出 Shizuku 授权请求，允许即可；若没弹，进应用设置里的 Shizuku/权限一节手动授权；
3. 回到工具页重新扫描 —— 之前标"无法删除"的私有目录缓存项，现在应能直接删除。

## 四、Shizuku 与 root 怎么选

| 维度 | Shizuku（无线调试） | Shizuku（root 启动） | 完全 root |
|---|---|---|---|
| 需要解锁/改系统 | 否 | 已 root 才行 | 是 |
| 重启后 | 需手动再启动 | 自动 | 自动 |
| 银行类应用兼容 | 正常 | 视 root 方案而定 | 部分检测拒跑 |
| 清理能力 | 接近 root | 接近 root | 最深 |
| 配对/启动复杂度 | 首次略繁琐 | 低 | 低 |

拿不准就先用无线调试方式试一周，够用就不用碰 root。授权生效与失败的排查见 [常见问题与故障排查.md](常见问题与故障排查.md)。
