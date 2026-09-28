# MiniTavern AI 模型接入与 API 配置

> 本篇讲模型这边怎么配：默认额度怎么用、自己的 API Key 怎么填、参数各管什么、不想花钱怎么走本地模型。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [Ollama本地模型离线聊天.md](Ollama本地模型离线聊天.md) · [OpenRouter免费模型接入方法.md](OpenRouter免费模型接入方法.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **MiniTavern 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/53fcf6923af5](https://pan.quark.cn/s/53fcf6923af5)

---

## 一、先说结论：不配也能聊

新用户自带免费额度和一套默认可用的模型，装完导张卡就能开聊，设置一个都不用碰。这篇是给两种人看的：对默认模型的回复不满意、想换更强模型的，以及手里已经有 API Key、想直接用自己账号的。

额度在「设置」页面看——用户名下方的用户类型与可用额度区域，剩余多少一目了然。

## 二、接入自己的模型：六步走

入口在底部导航第三个 Tab「设置」→「配置LLM」。

1. 点「提供商」下拉，选一家。可选的有：Nvidia、OpenAI、Anthropic、Google、Deepseek、Tencent、OpenRouter，以及「Other LLM」（OpenAI 兼容接口，兼容 Ollama）。
2. 在「API Key」框里粘贴你的密钥。
3. 点「Model」下拉加载模型列表，选一个。官方教程给的推荐口径：deepseek 消耗低，gemini 效果好但消耗高。
4. 点「测试连接」，等结果。
5. 显示「API 连接测试成功」后，「保存设置」按钮才会解锁，点保存。
6. 之后每次开新对话都用这套配置；聊天页里也可以快捷切换模型。

没有 Key 的，各家申请入口：

| 提供商 | 申请地址 |
| --- | --- |
| OpenAI（ChatGPT） | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |
| Anthropic（Claude） | [console.anthropic.com](https://console.anthropic.com/) |
| Google（Gemini） | [aistudio.google.com/app/api-keys](https://aistudio.google.com/app/api-keys) |
| Nvidia | [build.nvidia.com](https://build.nvidia.com/) |
| OpenRouter | [openrouter.ai/settings/keys](https://openrouter.ai/settings/keys) |

注意某些模型服务有地区限制（官方教程点名了 Gemini 与 Claude 的部分区域限制）：如果你所在网络的 IP 被归入受限清单，这家提供商的模型无论怎么配都用不了，换提供商或换网络环境再试。

## 三、三个默认参数各管什么

在「配置LLM」页切换到默认设置选项卡：

| 参数 | 默认值 | 作用与注意 |
| --- | --- | --- |
| 温度（Temperature） | 1 | 范围 0–2，越高回复越发散、越低越收敛。部分新模型（官方举例 Kimi K3）已不支持温度参数，调了不生效属正常，App 会自动处理 |
| 最大 Tokens（Max Tokens） | 4096 | 单次回复的长度上限。值越大消耗额度越快，且不能超过所选模型自身的上限 |
| 启用流式传输 | 关 | 开启后回复逐字蹦出来，不用干等整段。iOS 某些版本曾在流式上有过体验问题，遇到发送失败可先回设置里关掉它再试 |

改的是「默认设置」，对之后新开的对话生效。

## 四、额度与倍率：为什么同样的对话有的耗得快

每个模型挂了倍率，模型副标题里的 `(2x)` 表示这一次请求要扣两倍额度。默认模型列表会随维护变动，当前选中的模型在可用模型列表右侧带对号。省额度的直觉做法：日常闲聊用低倍率模型，重要剧情再切高倍率模型。

除免费额度与自己的 API Key 之外，应用还有订阅会员（专享会员模型、额度折扣、去广告、卡槽不限量）与一次性买断的 Ultra 版本，是否值得按你的用量自己算，本文不做推荐。

## 五、自定义 OpenAI 兼容接口

选「Other LLM」时填的是接口地址而不是提供商名：

- LLM URL 填 OpenAI 兼容的接口根地址，例如 `https://api.openai.com/v1`。
- 填完 URL 和 Key 后点模型列表按钮自动拉取可用模型。
- 中转站、Kimi、以及电脑上跑的 [Ollama本地模型离线聊天.md](Ollama本地模型离线聊天.md)，走的都是这条路。
- 想零成本试云端模型，按 [OpenRouter免费模型接入方法.md](OpenRouter免费模型接入方法.md) 接它目录里的 `:free` 档模型。

## 六、密钥与数据：官方隐私口径

官方隐私政策（截至 2026-09 的更新版）里有几条对日常使用有实际影响：

- **不需要注册账号**，不登录也能用导入卡、配模型、聊天这些核心功能。
- 免费用户**不收集任何数据**；升级付费后也只用设备标识做购买校验（哈希/令牌化存储，保留期为许可期加 90 天）。
- **API 密钥加密后只存你手机本地**，官方侧没有你的密钥；请求直接发给你选的服务商，应用不经手内容（会员模型除外——那部分经官方服务器转发，官方口径为不记录内容日志）。
- 导入的卡文件与聊天记录不上传，删除应用等于清空全部数据——重要内容先导出，政策原文以[官方隐私政策页](https://mini-tavern.com/tutorial/zh-CN/legal/privacypolicy.html)当时版本为准。

排查思路汇总在[常见问题排查.md](常见问题排查.md)。
