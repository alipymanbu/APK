# 手机当服务器SSH远程连接

> 本篇讲怎么用 ZeroTermux 在安卓手机上开 SSH 服务，让你从电脑直接连进手机敲命令：装 OpenSSH、设密码、启动服务、从电脑连接、常见报错怎么解。
> **相关文档**：[界面菜单与常用功能入门](界面菜单与常用功能入门.md) · [Linux发行版安装与容器切换](Linux发行版安装与容器切换.md)

---

> [!IMPORTANT]
> **ZeroTermux 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/13aebdc93002](https://pan.quark.cn/s/13aebdc93002)

---

## 一、这是什么场景

在手机小键盘上敲长命令很难受。把 SSH 服务开起来后，你可以在电脑的终端里远程操作手机上的 ZeroTermux，手机就成了一台挂在小局域网里的 Linux 服务器——跑下载、编译、定时脚本都方便。这套用法与 Termux 原版完全相同（ZeroTermux 基于 Termux），流程以 Termux Wiki 的 Remote Access 页为基准。

只对自己的设备做这件事，全程在**可信的局域网**里操作；不要把 8022 端口暴露到公网。

## 二、手机端：安装与启动

在 ZeroTermux 终端里依次执行：

1. 更新并安装 OpenSSH：

```bash
pkg update
pkg install openssh
```

2. 设置连接密码（输入时屏幕不显示，输完回车再输一遍）：

```bash
passwd
```

注意：`passwd` 不会问旧密码，忘了随时重跑重设。

3. 记下两个连接要用的信息——用户名和手机 IP：

```bash
whoami
ip addr show wlan0
```

`whoami` 输出形如 `u0_a123`，这就是你的用户名；`wlan0` 那一段里 `inet` 后面的地址（形如 `192.168.x.x`）就是手机 IP。

4. 启动 SSH 服务：

```bash
sshd
```

启动后没有成功提示，不报错就是在跑了。Termux/ZeroTermux 里 SSH 服务**默认端口是 8022**，不是常规的 22。

## 三、电脑端：连接

电脑和手机要在**同一个局域网**。Windows 自带的 PowerShell 或 CMD 里直接执行：

```bash
ssh u0_a123@192.168.x.x -p 8022
```

把用户名与 IP 换成上一步记下的值。第一次连接会问 `Are you sure you want to continue connecting`，输 `yes`，然后输入第二步 `passwd` 设的密码。登录成功后，你看到的提示符就是手机上的 ZeroTermux 终端。

停掉服务在手机端执行：

```bash
pkill sshd
```

## 四、连不上时的排查

| 现象 | 先查什么 |
| --- | --- |
| `Connection refused` | 手机端 `sshd` 起了没有；端口是不是写了 8022 |
| `Permission denied` | 手机端重跑 `passwd` 再试；用户名是否用的 `whoami` 的输出 |
| 连接超时 | 电脑和手机是否在同一 Wi-Fi；路由器开了「AP 隔离」的话设备间互 ping 不通，关掉它 |
| 连上一会儿就断 | 手机息屏后省电策略杀后台，按 [常见报错与问题排查](常见报错与问题排查.md) 第五节的做法关掉电池优化，屏幕别锁太快 |

想在 ZeroTermux 每次打开后自动有 SSH，可以把 `sshd` 追加到 `~/.bashrc`；ZeroTermux 的 ZT 功能里也有「开机启动」入口可以配合使用。

## 五、跟容器里的 SSH 别搞混

本篇开的是 **ZeroTermux 本体**的 SSH。你在 [Linux发行版安装与容器切换](Linux发行版安装与容器切换.md) 里装了 Ubuntu/Kali 容器的话，容器**内部**的 SSH 是另一套：要在容器里再装 `openssh-server` 并用 `service ssh start` 启动（容器里没有 `systemctl`）。两套服务、两个端口互不相干，别混着排障。
