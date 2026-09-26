# JanitorAI 模型与API设置教程

> 讲清默认模型怎么用、什么时候需要外接 API、接上了怎么填怎么测。
> **相关文档**：[创建AI角色教程.md](创建AI角色教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **JanitorAI 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e46d748e0a7a](https://pan.quark.cn/s/e46d748e0a7a)

---

## 一、先说结论：多数人不用碰这一页

JanitorAI 自带内置模型 **JanitorLLM**，默认就是它，不要密钥、不要注册别的平台，登录就能聊。免费额度对轻度聊天够用。**你什么都不配，也不会出现「连不上模型」这个状态** —— 所以真遇到发不出消息，先按 [常见问题与故障排查.md](常见问题与故障排查.md) 查网络和服务，别一上来就折腾 API。

需要配外接模型的典型情形只有两种：想要更强的文笔与更长的记忆窗口；或者内置模型被限流（报 429）时想换个通道继续。

## 二、可选的模型通道

| 通道 | 密钥从哪来 | 特点 |
| --- | --- | --- |
| JanitorLLM（默认） | 不需要 | 免费、开箱即用；高峰期可能排队 |
| OpenAI | `platform.openai.com` | 稳定，按量付费，每条消息有成本 |
| Claude | 官方控制台 | 长文与文笔见长，按量付费 |
| DeepSeek | `platform.deepseek.com` | 价格低，充值门槛也低 |
| OpenRouter | `openrouter.ai` | 一个密钥能选多家模型，有免费档 |

成本没有标准答案（随模型与消息长度变），开跑前自己在对应平台的定价页核一遍，别照抄别人帖子的数字。

## 三、配置步骤（以 OpenRouter + DeepSeek 为例）

1. 去 [openrouter.ai](https://openrouter.ai) 注册，进 Profile → **Keys** 建一个密钥并复制保存（当密码对待，别截图发群里）。
2. 登录 JanitorAI，打开任意聊天，右上角齿轮 → **API Settings** → **Add Configuration**。
3. 按下表填：

```text
Config Name:   My Free DeepSeek
Model Name:    deepseek/deepseek-chat-v3-0324:free
Proxy URL:     https://openrouter.ai/api/v1/chat/completions
API Key:       （粘贴你刚复制的密钥，前后别带空格）
Custom Prompt: 留空
```

4. 点 Add → **Save Settings**，刷新页面。
5. 发一条测试消息。通了，聊天窗口顶部的模型切换器里就能看到你这个配置，之后每局聊天都可以当场切换用哪个模型。

`Model Name` 与 `Proxy URL` 要逐字复制 —— 拼错不会报「格式错误」，只会连接失败，排查起来很绕。

## 四、配完不通：三个检查点

1. **密钥无效 / 余额不足**：回平台控制台看密钥状态和余额，免费档模型也要求账户里有最低充值。
2. **报 429 / rate limit**：是你所选通道的限流，不是 JanitorAI 挂了。等一会儿、换条通道，或临时切回 JanitorLLM。这与「整站打不开」是两回事，区分方法见 [常见问题与故障排查.md](常见问题与故障排查.md)。
3. **手机客户端上找不到这些设置**：模型配置在网页版改最省事，保存后客户端同步生效；客户端内置的受限模式相关内容也一样只能在网页版调整。

配置本身存在账号里（网页版），换设备不用重填。还没装客户端的话，安装包在 [下载与安装教程.md](下载与安装教程.md) 里写了怎么拿。
