# MiniTavern 接 Ollama 本地模型离线聊天

> 本篇讲不买 API 也能聊的路子：在电脑上跑 Ollama，手机上的 MiniTavern 通过局域网连它，全程免费、断网也能用。
> **相关文档**：[AI模型接入与API配置.md](AI模型接入与API配置.md) · [常见问题排查.md](常见问题排查.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **MiniTavern 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/53fcf6923af5](https://pan.quark.cn/s/53fcf6923af5)

---

## 一、这条路适合谁

手里没有 API Key、不想按额度付费，或者单纯想在没网的环境（通勤、出差）继续聊——那就把模型跑在自家电脑上。前提只有两条：家里有一台能装 Ollama 的电脑（Windows / macOS / Linux 都行），以及使用时手机和电脑连着**同一个 Wi-Fi**。

原理一句话：Ollama 在电脑上开一个模型服务，MiniTavern 在设置里把它当成一个「其他 LLM」来连。聊天数据照旧只存手机本地。

## 二、电脑端：装 Ollama 并对局域网开放

1. 到 [ollama.com](https://ollama.com/) 下载安装；macOS 也可以用 Homebrew：`brew install ollama`。
2. 拉一个小模型练手（约 1GB，足够验证链路）：

   ```bash
   ollama pull deepseek-r1:1.5b
   ```

3. 把服务暴露到局域网。装了图形客户端的，在 Ollama 设置里开启「Expose to the Network」；纯命令行的先停掉旧进程再开：

   ```bash
   pkill ollama
   OLLAMA_HOST=0.0.0.0:11434 ollama serve
   ```

   不做这一步时 Ollama 只监听 `127.0.0.1`，手机是连不上的——这是最常见的连接失败原因。
4. 查电脑的局域网 IP，一般以 `192.168` 开头。macOS 在 系统设置 → 网络 → Wi-Fi → 详细信息 → TCP/IP 里看 IPv4；或用命令：

   ```bash
   ifconfig | grep "inet " | grep -v 127.0.0.1
   ```

Windows 下同样思路：确认 Ollama 在运行（任务栏图标），再用 `ipconfig` 看 IPv4 地址。

## 三、手机端：MiniTavern 里连上去

1. 设置 → LLM 设置 → AI 服务商 → 选「其他」。
2. Base URL 填（把 IP 换成你电脑的）：

   ```text
   http://192.168.1.2:11434/api
   ```

   三个硬性要求：用 `http` 不是 `https`、端口是 `11434`、末尾必须带 `/api`。
3. 点「获取模型列表」，选中你刚拉的那个模型。
4. 点「测试连接」，成功后保存。之后聊天页里切换到这个模型即可。

连不上的按顺序查：Ollama 是否在运行 → 手机电脑是否同一 Wi-Fi → 是否做了第二步的局域网暴露 → Base URL 是否一字不差（`http` + `11434` + `/api`）。路由器开了「AP 隔离」的家庭网络也会连不通，去路由器设置里关掉。

## 四、日常使用的常用命令

```bash
# 查看已下载的模型
ollama list
# 下载模型
ollama pull deepseek-r1:1.5b
# 以本机监听启动（手机连不了）
ollama serve
# 以局域网可访问方式启动
OLLAMA_HOST=0.0.0.0:11434 ollama serve
# 停止服务
pkill ollama
```

## 五、两个提醒

- **模型大小量力选**。`1.5b` 这类小模型验证链路没问题，但角色扮演的对话质量明显偏弱；官方教程的建议是上 7B 及以上的模型，电脑显存或内存跟不上就会很慢。
- **不止 Ollama 一条路**。LM Studio、KoboldCpp 这类本地推理工具同样能通过「Other LLM」的 OpenAI 兼容接口接进来，思路与端口替换即可，具体以各自工具的接口说明为准。没有电脑、只想零成本用云端模型的话，[OpenRouter免费模型接入方法.md](OpenRouter免费模型接入方法.md) 那条路不需要额外设备。配好后如果对话出问题，排查入口在[常见问题排查.md](常见问题排查.md)；想换回云端模型，看[AI模型接入与API配置.md](AI模型接入与API配置.md)。
