# Cinemore 私有云 Docker 部署与远程访问

> 本篇讲在 NAS 上部署 Cinemore 私有云服务端的完整过程：Docker 安装、端口与卷映射、初始化设置，以及在外面连回家所需的条件。
> **相关文档**：[连接NAS与添加媒体源.md](连接NAS与添加媒体源.md) · [常见问题与报错排查.md](常见问题与报错排查.md)

---

> [!IMPORTANT]
> **Cinemore 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7f7a6d021d59](https://pan.quark.cn/s/7f7a6d021d59)

---

## 一、部署前确认两件事

- NAS 上有可用的 Docker 环境（群晖、飞牛等主流系统的套件中心都提供）；
- 想在**外网远程访问**的话，先确认家里宽带和手机端都有公网 IPv6——这是官方远程访问的硬条件，部署前就查清楚，免得部署完才发现连不出去。可以用 `test-ipv6.com` 这类在线检测页分别测 NAS 所在网络和手机网络。

只在家里局域网用的话，不需要 IPv6，也不需要公网 IP。

## 二、命令行安装

终端里执行（路径换成你自己的实际路径）：

```bash
docker run -d \
  --name cinemore-server \
  -p 8000:8000 \
  -v /path/to/data:/app/data \
  -v /path/to/media:/media \
  cinemore/cinemore-server:latest
```

两个映射是关键，含义如下：

| 容器内路径 | 放什么 | 示例 |
| --- | --- | --- |
| `/app/data` | 配置文件、数据缓存（刮削结果、账号数据都在这） | `/volume3/medias/cinemore:/app/data` |
| `/media` | 媒体文件目录 | `/volume3/medias:/media` |

`/media` 支持精细化映射，比如只挂剧集和电影两个子目录：

```bash
-v /volume3/medias/tv:/media/tv
-v /volume3/medias/movies:/media/movies
```

映射名也可以直接用中文（`/media/电视`、`/media/电影`），在 App 里看起来更直观。

`/app/data` 单独映射到一个独立目录还有个好处：以后删掉容器重建，刮削数据和配置还在。

## 三、图形界面安装（套件方式）

不方便敲命令的，走 Docker 图形界面：

1. 在镜像仓库里搜「cinemore」，下载 `cinemore/cinemore-server` 镜像；下载慢就先给 Docker 配置镜像加速源；
2. 在镜像详情页点「运行」；
3. 创建容器时填写端口和卷映射，对照上面表格里的两个路径填。

端口统一用容器内 `8000`，宿主机端口自己定（想用 8080 就映射 `8080:8000`）。

## 四、转成 Compose

官方提供了 compose 模板，最简的 SQLite 版本如下，把 `/自定义` 换成实际路径后 `docker compose up -d`：

```yaml
name: cinemore

services:
  cinemore-server:
    image: cinemore/cinemore-server:latest
    container_name: cinemore-server
    ports:
      - "8080:8000"
    volumes:
      # 配置文件目录映射
      - /自定义:/app/data
      # 媒体文件目录映射
      - /自定义:/media
    restart: unless-stopped
```

影库规模大、多用户场景，官方另有「独立数据库版本」模板：加一个 `postgres:17` 服务，主服务通过 `DB_TYPE=postgres`、`POSTGRES_HOST`、`POSTGRES_PASSWORD` 等环境变量连接。完整模板以官方 Docker 安装文档为准，不必自己拼。

## 五、服务端初始化

容器跑起来后，浏览器访问 `NAS的IP:8000`（或你自己映射的端口）：

1. **设置用户名和密码**：这是私有云的账号，手机端连接时也用它；
2. **添加文件源**：按实际情况选，以 Samba 为例——填自定义名称、主机地址、用户名密码，点连接，勾选要入库的文件夹。注意：如果选本地添加时**没出现文件夹列表**，说明第二步的卷映射有误，回 Docker 设置里排查 `/media` 那条映射；
3. **进入系统**：点「进入系统」，或直接用手机 App 扫码登录。进后台的任务栏能看到刮削任务正在进行；
4. **刮削完成**：媒体库里能看到全部影片；没识别出来的会进「未匹配」，已刮削但信息有误的可以点开手动修正——具体操作见 [媒体库使用与匹配纠错.md](媒体库使用与匹配纠错.md)。

## 六、手机怎么连

服务端就绪后，手机 App 开启「私有云服务」，用局域网发现（输账号密码或私有云短码）、扫码绑定、账号登录三种方式任选其一连接，操作细节见 [连接NAS与添加媒体源.md](连接NAS与添加媒体源.md)。

## 七、远程访问：三个条件缺一不可

官方对「在外面连回家里的 NAS」列了三个前提：

1. Docker 使用 **host 模式**（bridge 模式下 P2P 用不了，上面命令行示例是端口映射写法；要远程访问就把容器改成 host 网络）；
2. 服务端的 **P2P 功能已开启**；
3. **NAS 和手机都处在 IPv6 网络下**——家里宽带要有公网 IPv6，手机在室外要走蜂窝网络的 IPv6，连的是别人家 Wi-Fi 就看对方网络。

两点预期管理：P2P 目前是官方标注的**实验性功能**，可能遇到问题，官方让用户在使用中反馈；远程访问的更多排查（比如手机端什么算「有 IPv6」）见 [常见问题与报错排查.md](常见问题与报错排查.md) 的远程访问一节。
