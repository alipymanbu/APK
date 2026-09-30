# BotHub 连自己电脑上的 Ollama 怎么连

> 手机里填了电脑的地址却连不上，多半不是填错，而是 Ollama 默认只肯让本机访问。本篇按平台给出放开监听、放行端口的做法与验证步骤。
> **相关文档**：[怎么添加模型服务和填写API密钥.md](怎么添加模型服务和填写API密钥.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **BotHub 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/67dbb4e6bbb9](https://pan.quark.cn/s/67dbb4e6bbb9)

---

BotHub 的模型服务列表里带 Ollama。这一种**不经过别人服务器**：模型跑在你自己电脑上，手机只是把问题发过去。共用一份模型能省掉手机端重复下载，也能让手机用上电脑的显卡。

连不上的原因基本集中在一处：**Ollama 默认只监听 `127.0.0.1`**，也就是只接受本机的请求。同一局域网里的手机对它是「外人」，自然连不上。这不是故障，是默认设置。

## 一、先分清两种「跑了」

| 你想做的 | 监听地址 | 手机上填什么 |
| --- | --- | --- |
| 只有电脑自己用 | 默认 `127.0.0.1:11434` | 不用填，手机连不上 |
| 让手机也能用 | 改成 `0.0.0.0:11434` | `http://<电脑局域网IP>:11434/v1` |

注意第二条：手机端要填的是**电脑的局域网 IP**（形如 `192.168.1.100`），填 `localhost` 或 `127.0.0.1` 指的都是手机自己，永远连不上。API 类型选 OpenAI 兼容。

## 二、放开监听（按你的系统选一段）

改的是 `OLLAMA_HOST` 这个环境变量，改完必须**重启 Ollama** 才生效。以下做法来自 Ollama 官方 FAQ。

**Windows**：

1. 点任务栏的 Ollama 图标，选 Quit，先把它退干净。
2. 搜索「编辑环境变量」，在**用户变量**里新建一条：变量名 `OLLAMA_HOST`，变量值 `0.0.0.0:11434`。
3. 从开始菜单重新启动 Ollama。

**Linux**（以 systemd 托管的安装方式为例）：

```bash
sudo systemctl edit ollama
```

打开的编辑器里加入这两行，保存退出：

```ini
[Service]
Environment="OLLAMA_HOST=0.0.0.0:11434"
```

再执行这两条让它生效：

```bash
sudo systemctl daemon-reload
sudo systemctl restart ollama
```

**macOS**（把 Ollama 当作应用运行时）：

```bash
launchctl setenv OLLAMA_HOST "0.0.0.0:11434"
```

执行完同样要重启 Ollama 应用。

## 三、放行防火墙的 11434

监听放开了、防火墙还拦着，表现一模一样。Windows 第一次启动 Ollama 时会弹「是否允许访问网络」，那一刻勾**专用网络**同意即可；当时点了拒绝或没弹过，就手动加一条入站规则（PowerShell 以管理员身份执行）：

```powershell
New-NetFirewallRule -DisplayName "Ollama local" -Direction Inbound -Protocol TCP -LocalPort 11434 -Action Allow -Profile Private
```

Linux 上用 ufw 的话：

```bash
sudo ufw allow 11434/tcp
```

## 四、三步验证，能定位到具体是哪一环

按顺序做，哪一步断了就是哪一环的问题：

1. **电脑本机通不通**：在电脑上执行 `curl http://localhost:11434`，返回 `Ollama is running` 说明服务在跑。
2. **局域网通不通**：查电脑的局域网 IP（Windows 执行 `ipconfig`，看「IPv4 地址」；macOS 与 Linux 用 `ipconfig getifaddr en0` 或 `ip addr`），然后**用手机浏览器**打开 `http://<那个IP>:11434`。看到同一句 `Ollama is running`，说明监听和防火墙都通了。
3. **模型列不出来**：上一步通了但 BotHub 里拉不到模型，检查手机上填的地址有没有**漏掉结尾的 `/v1`**，以及 API 类型是不是 OpenAI 兼容。

第 2 步打不开时，先确认手机和电脑连的是**同一个 Wi-Fi**。有些路由器开了「AP 隔离 / 客户端隔离」，同一 Wi-Fi 下的设备也互相不通，这种情况要在路由器设置里关掉该选项，或把电脑改用网线接同一台路由。

## 五、开之前必须知道的一条安全边界

**11434 默认没有身份认证。** 谁能访问这个端口，谁就能读走、删掉你的模型文件，还能白用你的机器跑推理。

所以：

- **只在你信任的家庭或办公局域网里开**，咖啡馆、酒店、公共 Wi-Fi 下别开。
- **绝对不要把 11434 做端口转发到公网**，也不要用公网 IP 直接填进手机。需要在外网用自己的模型，走 VPN 回到家里网络，比裸露端口安全得多。
- 不想长期开着对外入口时，把 `OLLAMA_HOST` 改回 `127.0.0.1` 并重启 Ollama 即可关掉。

如果你只想在家里的 Wi-Fi 下用，第二节那套设置就是合适的范围；剩下的连接问题按 [常见问题与排查.md](常见问题与排查.md) 第三节的状态码对照处理。填地址、密钥的一般规则见 [怎么添加模型服务和填写API密钥.md](怎么添加模型服务和填写API密钥.md)。
