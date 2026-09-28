# OpenClaw 切换 AI 模型与 API Key 配置

> 初始化之后想换模型、加服务商、接本地模型，或者遇到「配了却不生效」，本篇按场景给出做法。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与排查.md](常见问题与排查.md) · [安全设置与权限管理.md](安全设置与权限管理.md)

---

> [!IMPORTANT]
> **OpenClaw 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/6235d6c4f99c](https://pan.quark.cn/s/6235d6c4f99c)

---

## 三种改法，按省事程度排

| 做法 | 影响范围 | 命令 |
|---|---|---|
| 会话内切换 | 只影响当前这个对话 | 对话里发 `/model 模型名` |
| 改默认模型 | 之后的会话都用新模型 | `openclaw models set 提供商/模型名` |
| 交互向导 | 服务商、API Key、默认模型一起改 | `openclaw configure` |

模型引用统一写成 `提供商/模型名` 的形式，例如 `deepseek/deepseek-v4-pro`。想看某个服务商下有哪些可选：

```bash
openclaw models list --provider deepseek
```

## 直接改配置文件

配置文件在 `~/.openclaw/openclaw.json`。改默认模型与提供商：

```json5
{
  models: {
    defaults: {
      provider: "anthropic",
      model: "claude-sonnet-4-20250514",
    },
  },
}
```

**改完必须重启 Gateway 才生效**：

```bash
openclaw gateway restart
```

## 常用服务商速查

截至 2026-09 的接法，字段以官方 Providers 文档为准：

| 服务商 | 关键配置 | 说明 |
|---|---|---|
| Anthropic | `ANTHROPIC_API_KEY` | 初始化向导里的默认选项之一 |
| Google | `GOOGLE_API_KEY` | Gemini 系列 |
| DeepSeek | `DEEPSEEK_API_KEY` | 先装官方插件：`openclaw plugins install @openclaw/deepseek-provider`，再 `openclaw onboard --auth-choice deepseek-api-key`，默认模型 `deepseek/deepseek-v4-pro` |
| Z.AI（GLM） | `ZAI_API_KEY` | `openclaw onboard --auth-choice zai-api-key`，模型引用走 `zai/*` |
| OpenRouter | `OPENROUTER_API_KEY` | 一个 Key 转多家模型 |
| Ollama | 自定义 baseUrl | 本地模型走 OpenAI 兼容端点（默认 `127.0.0.1:11434`），字段以官方 Ollama 页为准 |

DeepSeek 有个时效点：旧模型 ID `deepseek-chat` / `deepseek-reasoner` 已于 2026-07-24 停用，配置里还挂着这两个的要换成 `deepseek/deepseek-v4-flash` 或 `deepseek/deepseek-v4-pro`。

## 自定义 OpenAI 兼容端点

中转、私有部署这类 OpenAI 兼容服务，在 `models.providers` 下自建一个条目，`baseUrl` 和 API Key 两个都要给：

```json5
{
  models: {
    providers: {
      "openai-compatible": {
        baseUrl: "https://api.example.com/v1",
        apiKey: "$MY_API_KEY",
      },
    },
  },
}
```

只填了 Key 忘了 `baseUrl`，启动时会直接报配置校验失败，报错里会点名缺哪个字段。

## 换了模型却不生效

按出现率排：

1. **当前会话还挂着旧模型**——改的是默认值，正在聊的这个会话不会自己换。对话里发 `/model 模型名` 切当前会话，或开个新会话；
2. **改完配置没重启 Gateway**——`openclaw gateway restart` 一下；
3. **`/model` 提示 not allowed**——配置里对可选模型设了限制，别硬切，走 `openclaw configure` 向导改。

API Key 属于敏感凭据，跟白名单一样别进截图和聊天记录；存放它们的状态目录的对待原则，见 [安全设置与权限管理.md](安全设置与权限管理.md)。装好新模型后第一次跑不通，也可以先翻 [常见问题与排查.md](常见问题与排查.md)。
