# Vectras VM QEMU 参数进阶与预装镜像开机

> 讲进阶玩法：预装镜像直接开机、QEMU 参数逐段拆解、引导包版本怎么配套、3dfx 驱动光盘怎么装，以及卡住了去哪问。
> **相关文档**：[创建虚拟机与参数设置.md](创建虚拟机与参数设置.md) · [性能调优与卡顿处理.md](性能调优与卡顿处理.md) · [ROM镜像下载与导入.md](ROM镜像下载与导入.md)

---

> [!IMPORTANT]
> **Vectras VM 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/51e5c3c31fef](https://pan.quark.cn/s/51e5c3c31fef)

---

## 一、预装镜像：跳过装机流程的捷径

手动装一遍系统（流程见 [创建虚拟机与参数设置.md](创建虚拟机与参数设置.md)）要经历分区、复制文件、多次重启，在手机性能下相当耗时。更快的方式是用**预装镜像**——别人已经装好系统甚至装好驱动的虚拟硬盘文件（常见格式 qcow2），放进指定目录、配上参数，开机直接进桌面。

项目贡献者 An Bui（引导包的维护者）维护了一个 Windows XP 全驱动示例：镜像自带 SP3 与驱动，开机即可用，要求手机有至少 4 GB 空闲存储。教程页在 [https://anbui.ovh/vectrasvm/wxp.html](https://anbui.ovh/vectrasvm/wxp.html)，配套的视频讲解也挂在那里。

## 二、路径与参数：把预装镜像跑起来

两步：

1. **把镜像文件放进应用数据目录**。示例里的路径是 `/storage/emulated/0/Android/data/com.vectras.vm/files/data/Vectras/XP/`，也就是手机存储里该应用数据文件夹的 `files/data/Vectras/` 之下；
2. **把参数整行粘贴进创建虚拟机时的 QEMU PARAMS 输入框**。贡献者给出的 XP 全驱动配置是：

```text
-m 512M -accel tcg,thread=multi -drive file=/storage/emulated/0/Android/data/com.vectras.vm/files/data/Vectras/XP/XPSP3VL.qcow2,aio=threads,cache=unsafe -device rtl8139,netdev=n0 -netdev user,id=n0 -vga vmware -boot menu=on -vnc :2
```

逐段拆开，抄别人的配置前先知道每段管什么：

| 参数段 | 管什么 | 换自己的镜像时改哪 |
| --- | --- | --- |
| `-m 512M` | 虚拟机内存，512 MB 是 XP 的稳妥值 | 按目标系统与手机空闲内存调 |
| `-accel tcg,thread=multi` | 开多线程加速，多核手机明显更快 | 不动，照抄 |
| `-drive file=…qcow2,…` | 虚拟硬盘的完整路径 | **改成你放镜像的实际路径** |
| `-device rtl8139 … -netdev user …` | 虚拟网卡与用户态网络 | 不动，照抄 |
| `-vga vmware` | 虚拟显卡 | 不动，照抄 |
| `-boot menu=on` | 开机时显示启动菜单 | 不动，照抄 |
| `-vnc :2` | 显示通道编号 | 与其他机器冲突时才改 |

改路径这一步最容易错：`file=` 后面的整段路径必须与你实际放置的位置逐字一致，多一个空格或少一级目录，开机就会报找不到硬盘。

## 三、引导包版本要与主程序配套

引导包（首次启动时装的运行环境）不是随便换的——**版本必须与主程序配套**。本套文档对应的 v2.9.5，官方仓库的进阶文档里明确标注配的是 QEMU 8.2.0-3dfx 引导包；其他主程序版本各有自己的配套引导包，完整对照与下载地址都在官方的 [ADVANCED.md](https://github.com/xoureldeen/Vectras-VM-Android/blob/master/ADVANCED.md) 里。

主程序升级后想换更新版本的 QEMU，官方也提供了终端升级脚本（同样是版本配套关系，脚本页面标注了各自要求的主程序版本）。动手前先核对自己当前的主程序版本号，对不上就先升级主程序。

## 四、3dfx 驱动光盘：老 3D 游戏的最后一块拼图

[性能调优与卡顿处理.md](性能调优与卡顿处理.md) 里讲过 3dfx 补丁的用途，这里补上它缺的一环：**驱动从哪来**。

维护者仓库里提供了 3dfx wrapper 的 ISO 镜像，用法是把 ISO 当光盘挂进虚拟机（挂载操作见 [ROM镜像下载与导入.md](ROM镜像下载与导入.md) 的导入流程），再在 Windows 里像当年装 Voodoo 显卡驱动一样安装它：

- **旧版封装**（对应 QEMU 8.2.0-3dfx，也就是 v2.9.5 这一代主程序用的）：[3dfx-wrappers-2.9.5.iso](https://github.com/AnBui2004/Vectras-VM-Emu-Android/blob/master/3dfx/old/3dfx-wrappers-2.9.5.iso)；
- **新版封装**（对应更新的 QEMU 版本，i686 与 i586 两种）：见维护者仓库的 [3dfx 目录](https://github.com/AnBui2004/Vectras-VM-Emu-Android/blob/master/3dfx/)。

装好驱动再开游戏，游戏才会真正走 3dfx 通道。具体哪些游戏受益、性能预期，见 [性能调优与卡顿处理.md](性能调优与卡顿处理.md) 第三节。

## 五、卡住了去哪问

项目有两个官方求助渠道，提问时带上你的主程序版本、手机机型与具体报错截图：

- **Discord 服务器**：[https://discord.gg/t8TACrKSk7](https://discord.gg/t8TACrKSk7)；
- **Telegram 讨论组**：[Vectras VM 讨论组](http://t.me/vectras%5Fvm%5Fdiscussion)。

另外，官方仓库的 [https://vectras.vercel.app/how.html](https://vectras.vercel.app/how.html) 汇总了基础教程，先查文档再提问能省一轮往返。
