# AnLinux 安装 Linux 系统的完整流程

> 本篇从选发行版到进入命令行一条线讲完；图形桌面在另一篇。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [Termux环境准备与报错处理.md](Termux环境准备与报错处理.md) · [桌面环境与图形界面.md](桌面环境与图形界面.md) · [常见问题与解决方法.md](常见问题与解决方法.md)

---

> [!IMPORTANT]
> **AnLinux 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e53d7558362d](https://pan.quark.cn/s/e53d7558362d)

---

## 一、流程总览

AnLinux 装 Linux 的思路：你在 AnLinux 里选好发行版和架构 → 它生成一条安装命令 → 你把命令粘到 Termux 里执行 → 脚本从网上下载发行版镜像并解压 → 以后用启动脚本进入系统。

整个过程不需要 Root，装的所有东西都待在 Termux 自己的目录里，不碰系统分区。

## 二、操作步骤

1. 打开 AnLinux，进入发行版选择（主页的启动器/仪表盘入口）
2. 选一个发行版 —— 第一次建议 Ubuntu 或 Debian，理由见[支持的Linux发行版一览.md](支持的Linux发行版一览.md)
3. 选与你手机匹配的架构；不确定就选 arm64，近几年的手机都是
4. 点 COPY，安装命令复制到剪贴板
5. 打开 Termux，粘贴，回车执行 —— 前提是 Termux 已经初始化过（更新软件源、装好 wget 和 proot）；新装的 Termux 直接粘命令会报 `wget: command not found`，先按[Termux环境准备与报错处理.md](Termux环境准备与报错处理.md)第二节的命令准备
6. 等脚本跑完：它会下载几百 MB 的镜像并解压，时间主要取决于网速
7. 跑完后 Termux 主目录里会多出发行版目录和启动脚本，Ubuntu 对应 `./start-ubuntu.sh`（其他发行版脚本名不同，Kali 是 `./start-kali.sh`，以脚本实际生成为准）

## 三、进入与退出

进入系统：

```bash
./start-ubuntu.sh
```

执行后命令行前缀会变（形如 `root@localhost`），此时已在 Ubuntu 环境里，可以正常用 `apt update`、`apt install` 装软件。

退出：输入 `exit` 回到 Termux；Termux 本身不用时再输一次 `exit` 关掉会话。

## 四、装的东西在哪、占多少空间

- 全部在 Termux 的主目录下：以 Ubuntu 为例，镜像解压成 `ubuntu-fs/` 目录，旁边是 `start-ubuntu.sh`
- 查看占用：`du -sh ~/ubuntu-fs`
- 多个发行版可以并存，每个都实打实占存储，装之前用 `df -h` 看一眼剩余空间

## 五、删除一个发行版

两种办法任选：

- AnLinux 里有对应的卸载入口，选择发行版生成卸载命令，粘到 Termux 执行
- 手动删：在 Termux 里执行 `rm -rf ~/ubuntu-fs ~/start-ubuntu.sh`（目录与脚本名随发行版变，Kali 对应 `kali-fs`、`start-kali.sh`）

删干净后重装也不冲突，脚本会重新下载镜像。

## 六、值得提前知道的几件事

- **网络是最大变数**：镜像从 GitHub 一类地址下载，直连慢或失败很常见；失败了重跑同一条安装命令即可，脚本会重来
- **进去就是 root 用户**：装软件不用 `sudo`；但命令没有「后悔键」，`rm` 类操作下手前想清楚
- **这不是双系统**：只是 PRoot 挂载出来的用户空间，性能有一定损耗，不能换内核，依赖内核特性的功能（如部分需内核模块的工具）用不了
- **没有 systemd**：进入系统后执行 `systemctl` 会报 `System has not been booted with systemd as init system`—— 这是所有 PRoot 方案的共同限制，不是装坏了；服务类软件只能手动启动或用发行版自带的替代脚本，报错原文与更多环境限制见[常见问题与解决方法.md](常见问题与解决方法.md)
- **第一次先别急着装桌面**：命令行跑通、能装软件了，再按[桌面环境与图形界面.md](桌面环境与图形界面.md)加图形界面，出问题时好定位是哪一步的锅

卡在某一步时，对照[常见问题与解决方法.md](常见问题与解决方法.md)里的现象清单排查。
