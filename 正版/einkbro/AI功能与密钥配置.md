# EinkBro AI功能与密钥配置

> EinkBro 内置 AI 能力：对着当前网页提问、总结长文、把文章读出声。这些功能要自己填 API 密钥才工作，本篇讲在哪填、填什么、以及不用付费 API 的替代路线。
> **相关文档**：[翻译功能设置.md](翻译功能设置.md) · [下载与安装教程.md](下载与安装教程.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **EinkBro 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/42e9618f7e03](https://pan.quark.cn/s/42e9618f7e03)

---

## 一、AI 能做什么

| 功能 | 作用 | 入口 |
|---|---|---|
| Chat With Web | 分屏打开聊天面板，针对当前网页提问 | 工具栏 Chat With Web 按钮 |
| Page AI | 对整页内容跑预设动作（如总结要点） | 菜单 → Content Adjustment → Page AI |
| TTS 朗读 | 把文章读出声 | 菜单 → Read content |
| 自定义 AI 动作 | 自己写提示词，在选中文字时调用 | 设置 → Gen AI → ChatGPT Action Definition |

没有密钥时，这些功能都点不动或直接报错——先做第二节。

## 二、配置 API 密钥（设置 → Gen AI）

EinkBro 支持三类 AI 提供方，填其中任意一个即可：

### OpenAI

- OpenAI API Key：填你的密钥；
- OpenAI Model Name：默认 `gpt-4.1`，可改成其他型号；
- 朗读用 OpenAI TTS：默认开，模型可选 `tts-1`、`tts-1-hd`、`gpt-4o-mini-tts`。

### Google Gemini

- Use Google Gemini Model：打开开关；
- Gemini API Key：填密钥；
- Gemini Model Name：默认 `gemini-2.5-flash`。

### 自建 / Ollama（OpenAI 兼容接口）

- Use Alternative Server：打开；
- Custom Host URL：默认是 `https://api.openai.com`，改成你的自建服务地址（本地跑 Ollama 就填它暴露的地址）；
- Alternative Model Name：填服务端的模型名。

这条路不按调用量向模型服务付费，代价是自己要跑得起模型服务、自己负责配置。密钥是你自己的账号资产，只存在本机设置里，用量与计费以各家服务后台为准。

## 三、Chat With Web：对着网页提问

1. 打开想问的页面，点 Chat With Web；
2. 浏览器进入分屏，聊天面板占据一侧，AI 已带着页面内容，直接问即可（「这篇文章的结论是什么」「把步骤列出来」）；
3. 聊天记录会保存，可在历史里回看。

新版本把自由式代理（可以连续多步操作、帮你整理书签、给站点写修补脚本）加进了这个面板；网盘的 14.6.0 里是基础的「带页面上下文聊天」，够用但不带那些扩展动作。

## 四、Page AI 与自定义动作

- **Page AI** 对整页内容执行固定动作，提示词在 设置 → Gen AI → Prompt for Full Webpage Content，默认是「用 50 个词总结」；
- **ChatGPT Action Definition** 里可以定义多个自定义动作，各自写系统提示词；定义好的动作会出现在**选中文字的长按菜单**里，选段文字就能跑（翻译风格、解释术语、改写等都靠这个实现）；
- 整页的 AI 处理走「Web Processing GPT Type」选的引擎，与总结引擎分开，可以一个用 OpenAI 一个用 Gemini。

翻译相关的 AI 配置（逐段翻译用 OpenAI/Gemini）与这里共用同一组密钥，用法见[翻译功能设置.md](翻译功能设置.md)。

## 五、朗读（TTS）

- 菜单 → Read content 开始朗读当前页正文；
- 默认走 OpenAI TTS（需要 OpenAI 密钥），模型与音色说明在 设置 → Gen AI 的 TTS 区块；
- 用 `gpt-4o-mini-tts` 模型时，可以在 Audio Output Instructions 里写音色指令（比如指定播报风格）。

## 六、填好密钥后还是报错

按顺序检查：

1. 密钥有没有多余空格或漏字符；
2. 对应引擎的开关有没有打开（用 Gemini 却没开 Use Google Gemini Model 是最常见的）；
3. 自建服务地址写没写协议头（`http://` 或 `https://`）；
4. 账号本身有没有额度、有没有配额限制，以服务后台为准。

还解决不了时，去项目的 GitHub Issues 搜报错关键词，反馈渠道见[常见问题排查.md](常见问题排查.md)。
