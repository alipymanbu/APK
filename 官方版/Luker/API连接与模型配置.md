# Luker API 连接与模型配置

> 本篇讲装好 Luker 之后必须做的一步：接入一个大语言模型 API，否则发消息不会有任何回复。包括支持哪些 API、密钥从哪拿、怎么配多个连接随时切换。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [记忆图与多Agent编排.md](记忆图与多Agent编排.md) · [常见问题与解决方法.md](常见问题与解决方法.md) · [角色卡导入与创建.md](角色卡导入与创建.md)

---

> [!IMPORTANT]
> **Luker 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/60ed690e3f09](https://pan.quark.cn/s/60ed690e3f09)

---

## 一、为什么装完还不能聊

Luker 自己不带 AI 模型。它是一个「前端」：你打的消息、角色卡的设定，由它组装成请求发给某个大语言模型服务，再把回复渲染成聊天界面。所以第一次用必须先告诉它三件事——用哪家 API、地址是什么、密钥是什么。没有可用的 API 就没有对话，这一步绕不过去。

你已经有的东西决定走哪条路：

- 手里有 OpenAI / Claude / Gemini 等官方 API 密钥 → 走「聊天补全」类连接；
- 用的是各类中转站、聚合站 → 一般选「自定义 OpenAI 兼容 API」；
- 想跑本地模型省 API 费用 → Ollama 这类本地模型服务通常装在电脑或 NAS 上，让手机上的 Luker 连它的地址使用。

## 二、支持的 API 类型

聊天补全（Chat Completion）是大多数人会用的模式，商业 API 基本都属于这类：

| API 提供商 | 说明 |
| --- | --- |
| OpenAI | GPT 系列模型 |
| Anthropic | Claude 系列模型 |
| Google AI Studio / Vertex AI | Gemini 系列模型 |
| OpenRouter | 一个 Key 聚合多家模型的中转服务 |
| 自定义 OpenAI 兼容 API | 任何兼容 OpenAI 接口格式的服务（各类中转站多属此类） |

文本补全（Text Completion）主要面向本地或自托管模型：

| API 提供商 | 说明 |
| --- | --- |
| KoboldAI | 本地运行的开源模型 |
| Ollama | 本地模型管理和推理工具 |
| llama.cpp / TabbyAPI | 本地推理后端 |
| Text Generation WebUI | Oobabooga 的 Web 界面 |

不确定选哪个就用「聊天补全」，它是最常用的模式。

## 三、配一个能用的连接

1. 点界面顶部的 **API 连接** 图标；
2. 选 API 类型（比如 Claude、OpenAI 或自定义兼容）；
3. 填 API 地址和密钥；
4. 点测试连接，通过后选一个具体模型就能开聊了。

密钥的获取入口（以官方页面为准）：

| 提供商 | 密钥获取 |
| --- | --- |
| OpenAI | [https://platform.openai.com](https://platform.openai.com) 创建 API Key |
| Anthropic | [https://console.anthropic.com](https://console.anthropic.com) 创建 API Key |
| Google | [https://aistudio.google.com](https://aistudio.google.com) 获取 API Key |
| OpenRouter | [https://openrouter.ai](https://openrouter.ai) 注册后获取 |

密钥填进连接配置后存在服务端，不会暴露在聊天界面上。用别人部署的 Luker 实例时注意：密钥存在对方服务器上，介意就自己部署或只用手机 APK 本机版。

## 四、连接管理器：存多套配置随时切换

连接管理器可以保存任意数量的连接配置，比如：

- 日常闲聊用一个便宜模型；
- 写长剧情时切到旗舰模型；
- 本地模型单独存一份。

新建配置的路径：打开设置面板 → 连接管理器 → 「新建配置」→ 填名字、选 API 类型、填参数 → 保存。切换时在下拉列表里点一下即可，不用重新填地址和密钥。

熟悉斜杠命令的话还有更快的写法：`/profile 名称` 直接切换，`/profile-list` 列出全部配置。

还有一类容易被忽略的调用方：记忆图、多 Agent 编排这些内置工具自己也会调用模型，同样走连接配置。给它们单独存一套便宜模型的配置，主聊天照常旗舰、幕后杂活用便宜的，长期跑剧情能省不少，具体开法和代价见 [记忆图与多Agent编排.md](记忆图与多Agent编排.md)。

## 五、切换 API 不会动你的预设

这是 Luker 和原版酒馆使用习惯上差别最大的地方：API 连接和聊天补全预设是分开的两套东西。连接决定「用哪个模型、走什么地址」，预设决定「提示词怎么写、采样参数是多少」。切换连接时预设保持不变，所以你可以：

- 同一套调好的预设，在 OpenAI 和 Claude 之间来回切，对比同一剧情的输出差异；
- 同一个 API，配几套不同文风的预设，聊天里一键换风格。

两边随意组合，不用像原版那样切换 API 时连预设一起被换掉。

## 六、连不上或没回复时

先看三处：密钥是否填错或多复制了空格、账户是否还有额度、模型名是否选对。还是不行就打开内置的请求检查器（Request Inspector），它能显示每次发给 API 的完整请求和返回内容，报错信息一目了然。更多排查思路见 [常见问题与解决方法.md](常见问题与解决方法.md)。

配好 API 之后，下一步就是选个角色开聊，见 [角色卡导入与创建.md](角色卡导入与创建.md)。
