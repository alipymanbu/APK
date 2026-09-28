# SukiSU-Ultra boot镜像提取与修补

> 本篇讲走「管理器修补镜像」这条刷入路径时的关键判断：改 boot 还是 init_boot、原厂镜像从哪来、以及刷错时的退路。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [GKI内核判断与刷入方式选择.md](GKI内核判断与刷入方式选择.md) · [常见问题与风险须知.md](常见问题与风险须知.md)

---

> [!IMPORTANT]
> **SukiSU-Ultra 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/026236f4a5a6](https://pan.quark.cn/s/026236f4a5a6)

---

## 一、改 boot 还是 init_boot

出厂搭载 Android 13 的设备，通用 ramdisk 从 boot 镜像里挪到了独立的 init_boot 分区（AOSP 官方文档说明）。对应到修补操作：

| 你的内核版本 | 该修哪个 |
| --- | --- |
| `5.15.xx-android13-…`、`6.x.xx-android14-…` 这类 | `init_boot.img` |
| `5.10.xx-android12-…` 及更早 | `boot.img` |

两个最容易踩的坑：

- **看内核的安卓标记，不是系统设置里的安卓版本**。系统显示 Android 13、内核却是 `android12-5.10` 的设备完全存在（系统升过级、内核没动）——这时按内核算，修 `boot.img`。给 `android12-5.10` 的设备刷 Android 13 的内核，等来的就是反复重启（KernelSU 官方 FAQ 明确给出过这个结论）。
- **KMI 要一致**。内核版本里 `w.x-androidYY-k` 这一段（如 `5.10-android12-9`）相同才算兼容；版本号第三段（SubLevel）不同不影响。安全补丁级别也别刷老的 —— 新设备有防回滚，补丁级别更旧的镜像可能直接起不来。

## 二、原厂镜像从哪来

修补的前提是手里有一份**和你当前系统版本完全一致**的原厂镜像：

- **从固件包提取**：到你的设备品牌官方固件渠道下载对应版本的完整包。新式固件包里镜像都打在 `payload.bin` 里，社区通用做法是用 payload-dumper-go 这类开源工具把它解开，取出 `boot.img` / `init_boot.img`；
- **已在 root 状态的设备**：可以试管理器或内核刷写工具提供的备份功能，把当前分区原样导出；
- **提取完先验证**：文件大小一般几 MB 到几十 MB，明显不对（几 KB 或 0 字节）说明没提成功，别急着刷。

这份原厂镜像同时是你唯一的后悔药 —— 刷坏了靠它救回来（见第五节）。

## 三、修补与刷回

1. 把原厂镜像传进手机存储；
2. 打开管理器，顶部安装入口 → 从存储选中镜像 → 生成修补后的镜像（一般在 Download 目录，文件名带随机后缀，以实际生成为准）；
3. 修补文件传回电脑，手机进 fastboot 模式（bootloader），执行刷入 —— 刷哪个分区就写哪个名字：

```bash
fastboot flash init_boot 修补文件名.img
# 或老布局设备：
fastboot flash boot 修补文件名.img
```

4. `fastboot reboot` 重启。

## 四、fastboot 常见报错

| 报错 | 多数情况的处理 |
| --- | --- |
| `partition does not exist` | 当前是 fastboot 模式但分区要在 fastbootd 里刷：执行 `fastboot reboot fastboot` 进入 fastbootd 再重刷 |
| `waiting for any device` | 电脑驱动问题：换数据线 / 换 USB 口 / 装好设备驱动后重试 |

完整刷入流程（含修补入口在哪）见[下载与安装教程.md](下载与安装教程.md)；你设备属于哪一类，先看[GKI内核判断与刷入方式选择.md](GKI内核判断与刷入方式选择.md)。

## 五、刷错了怎么退回来

开机卡住时，进 fastboot 把**原厂镜像**刷回同一分区即可回到没刷过的状态：

```bash
fastboot flash init_boot 原厂init_boot.img
# 或对应分区
```

这也是为什么第二节反复强调「先有一致版本的原厂镜像再动手」—— 没有它，这个退路就不存在。更多故障排查见[常见问题与风险须知.md](常见问题与风险须知.md)。
