# BotGem API 密钥配置教程

> 讲怎么在 BotGem 里接入 OpenAI、Claude、Gemini、DeepSeek 等服务商的 API 密钥，配完才能正常聊天。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与报错排查.md](常见问题与报错排查.md) · [语音对话与思考模式玩法.md](语音对话与思考模式玩法.md)

---

> [!IMPORTANT]
> **BotGem 安装文件资源（夸克网盘）**：https://pan.quark.cn/s/49759d2b8b48

---

## 一、先搞清楚 BotGem 的收费逻辑

BotGem 本身分免费版与 Pro 版：免费版覆盖基础聊天，Pro 是**一次性买断**（不是订阅，各应用商店价格以商店页面为准），解锁高级功能。但无论用哪一版，**模型的调用费都不由 BotGem 收** —— 你用 GPT 就在 OpenAI 充值、用 Claude 就在 Anthropic 充值，账单直接来自那家服务商。

所以配密钥前你要有两样东西：

1. 一家服务商的账号（至少一家）；
2. 该账号里有可用余额或额度。

BotGem 不代售任何 API 额度，密钥得你自己去官方平台申请。

## 二、配置入口与通用流程

所有服务商走同一条路径：

1. 打开 BotGem，点**设置**（齿轮图标）；
2. 进入**服务提供商**；
3. 从列表里找到你要用的服务商；
4. 粘贴你的 API 密钥；
5. 点**保存**。

之后回到聊天界面，顶部会显示当前使用的模型，直接发消息即可。

## 三、各服务商密钥去哪申请

| 服务商 | 支持模型 | 密钥申请入口 |
| --- | --- | --- |
| OpenAI | GPT 系列 | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |
| Anthropic | Claude 系列 | Anthropic Console |
| Google | Gemini 系列 | Google AI Studio |
| Groq | Groq 加速的开源模型 | Groq 官方控制台 |
| Deepseek | DeepSeek 系列 | DeepSeek 开放平台 |
| Volcengine（火山引擎） | 火山方舟模型 | 火山引擎控制台 |
| Azure | OpenAI 系列（走 Azure 部署） | Azure 门户 |
| Ollama | 本地模型 | 无需密钥，见第六节 |

以 OpenAI 为例的完整步骤：

1. 打开 [OpenAI 平台的密钥页](https://platform.openai.com/api-keys) 并登录；
2. 点「创建新的密钥」，起个名字（可选）后确认创建；
3. **立刻复制** —— 密钥只在创建那一刻完整可见，关掉页面就再也看不到了；
4. 回到 BotGem 的服务提供商列表找到 OpenAI，粘进去，保存。

其余各家流程相同：都是「官方控制台创建密钥 → 复制 → 粘进 BotGem」。

## 四、配置时的常见细节

- **余额与密钥是两回事**：密钥创建成功不代表能用，账号里没充值/没额度照样报错；
- **多家可以同时配**：列表里填好几家后，在聊天界面顶部随时切换模型；
- **换密钥就重新粘贴覆盖**：保存即生效，不需要重启应用；
- **密钥别外发**：它等同于你账户的扣费凭证，泄露后别人可以用你的余额。

若保存后发消息报「API 密钥无效」，按 [常见问题与报错排查.md](常见问题与报错排查.md) 逐条排查。

## 五、OpenAI 兼容服务怎么接

除官方列表里的服务商，任何**提供 OpenAI API 兼容接口**的服务（各种中转、自建网关、云厂商兼容端点）都能接进 BotGem，步骤：

1. 服务提供商列表里选择 OpenAI 兼容服务的添加入口；
2. **API Server** 填对方给的接口地址（通常形如 `https://xxx.com/v1`，以服务商文档为准）；
3. **API Key** 填对方发你的密钥；
4. **Models** 点刷新拉取模型列表，拉不到就手动输入模型名；
5. 点**检查连接**，显示连接成功后保存。

注意 BotGem 走的是标准 Chat Completions 格式（`/v1/chat/completions`），只提供 Responses 类接口（`/v1/responses`）的端点接不上 —— 这种情况下换一家兼容 Chat Completions 的服务商。

## 六、没有密钥也能跑：Ollama 本地模型

如果你既不想充值、又希望对话内容不出设备，可以让 BotGem 连本机的 [Ollama](https://ollama.com/)：

1. 先在电脑上装好 Ollama 并拉取至少一个模型；
2. BotGem 服务提供商里选择 Ollama，指向本机端口（默认 `http://localhost:11434`，以 Ollama 实际配置为准）；
3. 保存后即可离线聊天。

代价是模型质量与速度取决于你机器的性能，且首次配置对新手有一定门槛；两种路线的取舍见 [语音对话与思考模式玩法.md](语音对话与思考模式玩法.md) 的隐私小节。
