# 自建ID与中继服务器教程

> 不走公共服务器：一条 Docker 命令起服务端，再把手机指到你自己的服务器。
> **相关文档**：[手机远程控制电脑教程.md](手机远程控制电脑教程.md) · [连不上与卡顿排查.md](连不上与卡顿排查.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **RustDesk 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/729b4a3aecb7](https://pan.quark.cn/s/729b4a3aecb7)

---

## 一、什么情况值得自建

RustDesk 默认用官方公共服务器撮合连接，轻度使用够用。出现这几种情况时，你会想换成自己的：

- 公共服务器连接慢、频繁提示连接错误；
- 你希望中继流量不过第三方机器；
- 所在网络封锁了默认端口（有用户反馈家用宽带封过 21115~21118 段端口，对策见第六节）。

自建只需要一台有公网 IP 的服务器，服务端是开源的（`github.com/rustdesk/rustdesk-server`），免费。

## 二、用 Docker 起服务端

在 Linux 服务器上装好 Docker 后，两条命令（`hbbs` 负责 ID 注册与打洞，`hbbr` 负责中继）：

```bash
sudo docker image pull rustdesk/rustdesk-server

sudo docker run --name hbbs -p 21115:21115 -p 21116:21116 -p 21116:21116/udp -p 21118:21118 -v `pwd`:/root -td --net=host rustdesk/rustdesk-server hbbs

sudo docker run --name hbbr -p 21117:21117 -p 21119:21119 -v `pwd`:/root -td --net=host rustdesk/rustdesk-server hbbr
```

起完用 `docker ps -a` 确认两个容器都是 `Up`。工作目录下会生成密钥文件，其中 **`id_ed25519.pub` 的内容就是待会要填进客户端的 Key**。

Windows 也可以当服务端：官方有 `RustDeskServer.Setup.exe` 安装包，装完在界面点 Start 即可，参数与上面一致。

## 三、放行端口

| 端口 | 协议 | 用途 |
| --- | --- | --- |
| 21115 | TCP | NAT 类型测试 |
| 21116 | **TCP + UDP** | ID 注册与心跳（UDP 别漏） |
| 21117 | TCP | 中继 |
| 21118 / 21119 | TCP | 网页客户端支持，不用网页版可不开 |

云服务器的安全组和系统防火墙**两处都要放行**，只开一处照样连不上。

## 四、手机端指向自己的服务器

在手机 RustDesk 里：**设置 → ID/中继服务器**，填写：

| 字段 | 填什么 |
| --- | --- |
| ID 服务器 | 服务器的域名或公网 IP（如 `你的域名:21116`，默认端口可省略） |
| 中继服务器 | 同一台机器即可；与 ID 服务器相同时可留空 |
| API 服务器 | 留空 |
| Key | `id_ed25519.pub` 的完整内容（加密需要，整段粘贴别带换行问题） |

点确定保存后会自动切到你的服务器。也可以走扫码配置：把 `config={"host": "你的主机", "key": "你的公钥"}` 这段生成二维码，用 RustDesk 的扫码按钮扫入。桌面端配置入口在 设置 → 网络 → ID/中继服务器（桌面端需先解锁网络设置）。

两端都指向同一台自建服务器后，连接方式与原来完全一样：输 ID、输密码。日常操作见[手机远程控制电脑教程.md](手机远程控制电脑教程.md)。

## 五、让服务端一直活着

服务器重启或容器崩了没自动拉起，所有客户端会突然集体连不上。三种保持方法按你的部署方式选：

- **Docker（docker run 起的）**：给两个容器加自启——`docker update --restart=always hbbs hbbr`；
- **Docker Compose**：在 compose 文件里给每个服务写 `restart: unless-stopped`，之后 `docker compose up -d` 即可；
- **裸二进制 + systemd**：写成 service 单元并 `systemctl enable hbbs hbbr`，让它们开机自启、崩溃后 5 秒自动重启。

排障时先看日志：`docker logs hbbs` / `docker logs hbbr`，端口没放通、Key 不匹配这类问题日志里都有明确报错；重启容器用 `docker restart hbbs hbbr`。密钥文件丢失会让所有客户端的 Key 失效，备份好放密钥的那个目录（`id_ed25519` 与 `id_ed25519.pub`）。

## 六、端口被封的替代方案

运营商封了 21115~21118 时，有两条实测过的路：

1. **换端口**：把服务端映射到 443 等常见端口（HTTPS 通常不会被封），客户端 ID 服务器里带上报的端口；
2. **强制走中继**：连接时在对方 ID 后加 `/r` 后缀（如 `123456789/r`），跳过直连尝试。此法来自第三方用户反馈，适用性以你的实测为准。

连不上时的完整排查顺序，见[连不上与卡顿排查.md](连不上与卡顿排查.md)。
