# ServerBox Monitor 部署与远程访问配置

> ServerBox 的桌面小部件、推送、网页面板，都依赖装在服务器上的 Monitor 组件。本篇讲它的安装命令、App 怎么以 Monitor 方式接入，以及 full_access 与 TLS 这些要想清楚再动的开关。
> **相关文档**：[桌面小部件与状态推送配置.md](桌面小部件与状态推送配置.md) · [状态监控与Docker进程管理.md](状态监控与Docker进程管理.md) · [添加服务器与SSH连接配置.md](添加服务器与SSH连接配置.md)

---

> [!IMPORTANT]
> **ServerBox 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/b974924a12ac](https://pan.quark.cn/s/b974924a12ac)

---

## 一、Monitor 在整个体系里的位置

ServerBox 本身只需要手机能 SSH 到服务器，服务器上什么都不用装。Monitor 是官方提供的服务端组件，装在你想持续盯着的机器上，负责记录指标、对外提供 HTTP API，还能托管一个网页面板。装上之后多出来三样东西：

- 不开 App 也持续积累的历史数据（App 内图表是连上才开始记录的，Monitor 没有这个限制）；
- 桌面小部件、推送、手表这类轻端的数据来源；
- 一个浏览器直接打开的服务器面板。

不装也不影响 App 内的 SSH、SFTP、容器管理这些常规功能。小部件怎么配见「桌面小部件与状态推送配置」。

## 二、安装：一条脚本命令

官方安装脚本按你服务器的 init 系统选一种跑法（以官方文档为准：[github.com/lollipopkit/flutter_server_box/blob/main/monitor/README_zh.md](https://github.com/lollipopkit/flutter_server_box/blob/main/monitor/README_zh.md)）：

```sh
# systemd 发行版：装成用户级服务，以你登录的账号运行
curl -fsSL https://raw.githubusercontent.com/lollipopkit/flutter_server_box/main/monitor/install.sh | sh -s -- install

# Alpine (OpenRC)：写 /etc/init.d 需要 root，但 agent 仍以你的账号运行
curl -fsSL https://raw.githubusercontent.com/lollipopkit/flutter_server_box/main/monitor/install.sh | sudo sh -s -- install

# 以 root 系统服务运行（两种 init 通用）
curl -fsSL https://raw.githubusercontent.com/lollipopkit/flutter_server_box/main/monitor/install.sh | sudo sh -s -- install --system
```

另外三条路：

- 机器离线或还没有对应 release：用 `SBM_INSTALL_PKG=/path/to/server-box-monitor` 指向自己构建的包再装；
- 习惯容器的走官方仓库里的 Dockerfile；
- 卸载与升级：把命令里的 `install` 换成 `uninstall` / `upgrade`。

装完核对两件事：

1. 配置文件 `config.toml` 在二进制旁边，全部配置项和注释在官方 `config.example.toml` 里——版本之间格式可能调整，升级 Monitor 后重新核对一遍；
2. agent 默认监听 `0.0.0.0:3770`，面板可用时也在这个地址。

## 三、App 侧怎么接

在 ServerBox App 里添加服务器时，除了常规 SSH 方式，还可以选 **monitor 方式**：这台「服务器」只通过 Monitor 的 HTTP API 取数据，不携带任何 SSH 凭据——适合不方便暴露 SSH 端口的主机，而且图表在首次连接前就已经有历史数据了。

以 monitor 方式接入后，能用哪些功能取决于 Monitor 配置里开了什么：

| 你要用的功能 | 需要的条件 |
| --- | --- |
| 状态、图表、历史曲线 | 只要能登录 Monitor |
| 进程、systemd、容器、snippet、电源操作 | `full_access` |
| 终端 | `full_access`，且终端默认关闭、需在配置里显式开启 |
| 文件浏览 | `[remote_access.fs]` 开启并配好 `roots` |

monitor 方式的边界也要知道：**SFTP 和端口转发不提供**——agent 没有任何端点把连接中继到别的地址。这两样还得用普通 SSH 方式添加同一台机器，两种方式可以并存。

## 四、full_access：想清楚再开

`full_access` 的实际含义：能登录 Monitor 面板的人，等于拿到了 Monitor 进程所属账号的 shell。

- 默认值跟随平台：Linux 默认开，macOS 与 Windows 默认关；
- 官方安装脚本默认装成用户级服务，就是为了把这个风险压在你的账号范围内；
- 以 root 运行 Monitor 的话，把 `full_access` 关掉（环境变量 `SBM_FULL_ACCESS=0/1` 也能控制）；
- 面板的首次使用提示可以关掉它，但**永远无法在面板里重新打开**——要再开得回配置文件改。

一句话判断：只给你自己用、跑在你自己账号下，开着方便；root 运行、面板密码弱，关掉。SSH 登录方式始终并存，关掉 full_access 不影响基础功能。

## 五、远程终端与明文传输的边界

- 网页终端（`[remote_access.terminal] enabled`）默认关闭，要显式开。开了之后面板里能打开网页 shell，会话断线后保留几分钟，手机切网可以接回同一个 shell；
- 终端拒绝跑在明文 HTTP 上——它的第一条消息就带着 SSH 密码。要么给 Monitor 配 TLS，要么同机反向代理；在 Tailscale 这类可信私有网络里，可以在配置中设 `allow_insecure = true`，但 **App 端还要对该 Monitor 连接单独开启「允许不安全 HTTP」，两端开关缺一不可**；
- 文件 API 同样要求 TLS，规则与上面相同；
- Monitor 首次连接会固定 sshd 的 host key，之后不匹配即拒绝，不会静默重置；要清除固定得手动删 `ssh_known_hosts` 里对应的记录。

## 六、装完后的检查清单

1. 浏览器访问 `http://服务器IP:3770`，有响应或能打开面板；
2. App 里以 monitor 方式添加这台机器，能看到状态图表与历史曲线；
3. 需要终端、文件浏览的，回 `config.toml` 把对应开关打开，再到 App 里验证；
4. 小部件的配置步骤见「桌面小部件与状态推送配置」——Monitor 装好是它的前置条件；
5. Monitor 与 App 的功能对应关系随版本演进，行为对不上时以官方 Monitor 文档为准。
