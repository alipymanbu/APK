# Aria2App DirectDownload 怎么设置

> DirectDownload 解决的问题：aria2 把文件下到了服务器上，你要把它们拉回手机。本篇讲服务器端的 HTTP 目录服务怎么起、App 端地址怎么填。
> **相关文档**：[怎么连接远程服务器.md](怎么连接远程服务器.md) · [连接失败与常见问题.md](连接失败与常见问题.md)

---

> [!IMPORTANT]
> **Aria2App 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/66c30f0e8792](https://pan.quark.cn/s/66c30f0e8792)

---

## 一、为什么需要它

RPC 只负责「控制」—— 下什么、暂停、删除。文件本体落在服务器磁盘上，Aria2App 拿不到。DirectDownload 是官方给的取回通道：在服务器上再起一个普通的 HTTP 文件服务，把下载目录暴露成网页目录列表，App 从那里把文件拉回手机。

所以整体结构是两件事叠加：

| 通道 | 端口（示例） | 作用 |
| --- | --- | --- |
| JSON-RPC | 6800 | 控制：加任务、暂停、改选项 |
| HTTP 文件服务 | 任选，如 8010 | 取回：从服务器下载已完成的文件 |

## 二、服务器端：起一个 HTTP 目录服务

官方 Wiki 列了多种做法，任选其一，服务的根目录必须是 aria2 的下载目录（`--dir` 指的那个）。

### 方式 A：Python 一行命令（最快）

在下载目录下执行：

```bash
python3 -m http.server 8010
```

- Linux 上可能要 `sudo`；Windows 首次运行会弹防火墙提示，选「允许访问」；
- 缺点：没有认证、不支持断点续传，适合纯内网临时用。

### 方式 B：作者提供的脚本（支持认证与断点）

devgianlu 写了一个现成脚本 [serve_http.py](https://gist.github.com/devgianlu/018b299f8817bf92350bf7bf70214e4d)（要求 Python 3.7+），支持 Authorization 头认证与 Range 断点续传：

```bash
wget https://gist.githubusercontent.com/devgianlu/018b299f8817bf92350bf7bf70214e4d/raw/serve_http.py
python3 serve_http.py
```

要开认证的话，编辑脚本里的两行改成你自己的账号密码：

```python
USERNAME = "username123"
PASSWORD = "password456"
```

### 方式 C：Apache（长期服务）

适合本来就跑着 Apache 的机器：在 `/etc/apache2/sites-enabled/` 下新建一个 `.conf`，监听一个自选端口、把 `DocumentRoot` 指到下载目录、允许目录索引（`Options +Indexes`），改完 `systemctl restart apache2`。目录权限不对会得到 403，检查 `chown` / `chmod`。Wiki 上的完整配置模板可直接照抄，细节以[官方 Wiki](https://github.com/devgianlu/Aria2App/wiki/Setup-DirectDownload) 为准。

## 三、App 端：把地址填进 profile

打开对应服务器的 profile，找到 DirectDownload 设置，地址填**指向目录列表的完整地址**。注意这里和 RPC 地址的填法不同 —— 这里要带协议和端口：

- 服务器 IP 是 `192.168.1.8`、脚本跑在 8010 → 填 `http://192.168.1.8:8010/`；
- 方式 B 开了认证的，App 端对应位置填上那组用户名密码；
- HTTPS 地址同样支持。

填完在 App 里选一个已完成的任务，拉取一次验证。

## 四、要不要暴露到公网

- 只在家/局域网用：什么都不用改，最安全；
- 想在外面随时取回：路由器把那个端口映射到服务器，服务器防火墙放行；
- 暴露公网就**必须**配认证（方式 B 或 Apache Basic Auth），否则等于把整个下载目录对全网开放。谨慎起见，也可以配 VPN 后再走内网地址，不直接暴露文件服务。

## 五、连不上时

文件服务地址浏览器里先开一遍 —— 浏览器能看到目录列表，Aria2App 就一定连得上；浏览器都打不开，问题在服务器端（服务没起、防火墙、端口映射），与 App 无关。其余通用排查见 [连接失败与常见问题.md](连接失败与常见问题.md)。
