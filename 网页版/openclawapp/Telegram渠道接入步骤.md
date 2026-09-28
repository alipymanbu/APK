# OpenClaw Telegram 渠道接入步骤

> 从零把一个 Telegram bot 接进 Gateway：建 bot、填 token、批准第一条私信、拉进群组，含群里收不到消息的处理。
> **相关文档**：[支持的消息渠道一览.md](支持的消息渠道一览.md) · [安全设置与权限管理.md](安全设置与权限管理.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **OpenClaw 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/6235d6c4f99c](https://pan.quark.cn/s/6235d6c4f99c)

---

## 为什么建议第一个接 Telegram

Telegram 的默认私信策略就是配对（pairing），bot token 免费、即办即用，整个流程 5~10 分钟能走完。前提只有一个：Gateway 已经跑起来（部署见 [下载与安装教程.md](下载与安装教程.md)）。

## 第一步：在 BotFather 建 bot

打开 Telegram，找 **@BotFather**（注意核对 handle 一字不差），发：

```text
/newbot
```

按提示起名、设用户名，BotFather 会回给你一串 bot token（形如 `123456789:ABCdef...`），复制保存。不想在聊天里操作的话，BotFather 也有网页版（Telegram 客户端里都能打开），在界面里建完同样复制 token。

## 第二步：把 token 填进配置

编辑 Gateway 的配置文件（`~/.openclaw/openclaw.json`），加上：

```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "你的bot token",
      dmPolicy: "pairing",
      groups: { "*": { requireMention: true } },
    },
  },
}
```

几个要点：

- Telegram 不走 `openclaw channels login` 这类登录命令，token 填进配置或环境变量后直接启动 Gateway；
- 环境变量兜底是 `TELEGRAM_BOT_TOKEN`，只对默认账号生效；优先级是配置里的 `tokenFile` > `botToken` > 环境变量；
- `groups` 里的 `requireMention: true` 让 bot 在群里只回应 @它 的消息，不搅局。

## 第三步：启动 Gateway、批准第一条私信

```bash
openclaw gateway restart
```

用你自己的 Telegram 账号给 bot 发一条消息——它不会回答，而是回你一个配对码。到 Gateway 主机上批准：

```bash
openclaw pairing list telegram
openclaw pairing approve telegram <配对码> --notify
```

码是 8 位大写、1 小时过期；过期了让对方再发一次消息就能拿到新码。批准机制详见 [安全设置与权限管理.md](安全设置与权限管理.md)。

## 第四步：拉进群组

群组要单独放行，需要两个 ID：

| 要什么 | 用在哪 | 怎么拿 |
|---|---|---|
| 你的 Telegram 用户 ID | `allowFrom` / `groupAllowFrom` | 给 @userinfobot 发条消息，它会回你数字 ID |
| 群的 chat ID | `channels.telegram.groups` 的键 | 看 `openclaw logs --follow` 里收到消息的 ID，或 Bot API 的 `getUpdates` |

把 bot 拉进群后，配置里这样写（超群组的 chat ID 是 `-100` 开头的负数，注意它放在 `channels.telegram.groups` 下面，不是 `groupAllowFrom`）：

```json5
{
  channels: {
    telegram: {
      groupPolicy: "allowlist",
      groupAllowFrom: ["你的用户ID"],
      groups: { "-100xxxxxxxxxx": { requireMention: true } },
    },
  },
}
```

拿不准配置对不对，在群里发 `/whoami@你的bot用户名`，bot 会回你当前的用户 ID 与群 ID，用来核对。

## 群里不说话：先查 Privacy Mode

Telegram bot 默认开着 **Privacy Mode**——群里只有 @它 的消息和命令它收得到，其他发言一律看不见。想让它读全部群消息，二选一：

- 在 BotFather 里对 bot 执行 `/setprivacy` 关掉；
- 或者把 bot 设为该群的管理员（管理员收全部消息）。

改完有个坑：**要把 bot 从每个群移出再重新拉进来**，Telegram 才会让新设置生效。另外 `/setjoingroups` 可以控制这个 bot 允不允许被拉群。

## 顺手：在 Telegram 里开控制台

在跟 bot 的私信里发 `/dashboard`，可以在 Telegram 里直接打开 OpenClaw 控制台（Mini App）。两个前提：Gateway 配了 Tailscale Serve/Funnel 这类 HTTPS 入口；你的用户 ID 在私信白名单里。在群里发它会提示你移步私信。

## 接完之后

群发消息收不到、配对码过期这类问题，集中整理在 [常见问题与排查.md](常见问题与排查.md)；其他渠道（WhatsApp、Discord、飞书等）的清单见 [支持的消息渠道一览.md](支持的消息渠道一览.md)。
