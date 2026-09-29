# JsxposedX AI 功能配置与使用

> 讲 JsxposedX 的 AI 功能怎么配置：填哪几项、Base URL 为什么总填错、连接测试失败先查什么，以及 AI 分析能给你什么。
> **相关文档**：[项目管理与快捷功能.md](项目管理与快捷功能.md) · [环境检测与运行模式.md](环境检测与运行模式.md) · [常见问题与注意事项.md](常见问题与注意事项.md)

---

> [!IMPORTANT]
> **JsxposedX 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/3e5935f68543](https://pan.quark.cn/s/3e5935f68543)

---

## 一、AI 功能在你的流程里处于什么位置

AI 在 JsxposedX 里是辅助角色：基于目标应用的 Manifest、文件结构、Dex 类信息、SO 与 JNI 信息做结构说明、筛可疑类与方法、给出分析线索，需要脚本时还能直接产出 Xposed 或 Frida 脚本草稿。

要用它，前提有三个：目标应用已加入项目、AI 配置已完成、AI 服务本身可达（环境检测面板的 AI 项为正常）。

## 二、配置步骤

1. 打开 JsxposedX 的设置页；
2. 进入「AI 配置」；
3. 新建一个配置（或编辑已有配置）；
4. 选择 API 类型；
5. 填 Base URL、API Key、模型名称；
6. 按需调整 Max tokens、Temperature、Memory rounds；
7. 点「测试连接」；
8. 通过后把这个配置设为当前配置。

## 三、各字段填什么

| 字段 | 说明 |
| --- | --- |
| 配置名称 | 用来区分不同服务商或模型，自己看得懂就行 |
| API 类型 | 请求协议类型，与你的服务商对上（OpenAI 兼容 / Anthropic 等） |
| Base URL | API 基础端点，**不是**服务商的官网首页或文档页 |
| API Key | 服务商给你的访问凭证 |
| 模型名称 | 模型 ID，必须是该服务实际支持的写法 |
| Max tokens | 单次回复的长度上限 |
| Temperature | 输出随机性 |
| Memory rounds | 上下文携带的对话轮数 |

## 四、Base URL 是最容易填错的一项

官方文档专门强调了这条：填 **API 基础端点**，不要填网页地址。

- OpenAI 兼容接口：通常是以 `/v1` 结尾的基础路径；
- **不要**把 `/chat/completions`、`/responses` 这类完整请求路径拼进去（服务商明确要求除外）；
- Anthropic 接口：填 Anthropic 基础端点或兼容端点，同样不要填控制台或文档页。

几个官方给的参考示例：

| API 类型 | Base URL 示例 | 模型名示例 |
| --- | --- | --- |
| OpenAI 兼容 | `https://api.openai.com/v1` | gpt-4.1-mini |
| OpenAI 兼容 | `https://api.deepseek.com/v1` | deepseek-chat |
| OpenAI 兼容 | `https://dashscope.aliyuncs.com/compatible-mode/v1` | qwen-plus |
| Anthropic | `https://api.anthropic.com` | claude-3-5-sonnet-latest |

## 五、测试连接失败，按顺序查

1. API 类型与服务商是否匹配；
2. Base URL 是不是真正的 API 端点（而不是网站首页）；
3. Base URL 里是否多拼了 `/chat/completions`、`/messages` 之类的完整路径；
4. 模型名称是否是该服务实际支持的模型 ID；
5. API Key 里是否混入了空格、换行，或前缀写错。

官方还内置了一个默认 AI 接口（沐雪 AI，`muxueai.pro`，官网首页有入口），不想自己找服务商的可以先用它跑通流程；你也可以按上表配置自己获取的第三方 AI 服务，没有强制限制。

## 六、参数怎么调更顺手

- 做分析与代码生成时，把 **Temperature 调低**，输出更稳定；
- 回复被截断就调大 **Max tokens**；
- 觉得 AI 记不住前面聊过的内容就调大 **Memory rounds**。

## 七、费用这件事心里有数

AI 调用涉及模型算力与接口成本：应用提供应用内购买项目，内置的默认接口按官方说明可能对相关服务收取合理成本费用；自配第三方接口时，花费按你与服务商的约定走。配置之前先看一眼价格页，避免跑大批量分析时产生意外开销。

配置完、测试也通过之后，回到目标应用的菜单里进「AI 分析」开始用，入口位置见 [项目管理与快捷功能.md](项目管理与快捷功能.md)。
