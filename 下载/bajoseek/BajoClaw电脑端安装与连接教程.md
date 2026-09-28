# BajoClaw电脑端安装与连接教程

> 想在手机 BajoSeek 里发一句话、让家里的电脑自动干活，要先在电脑上装好 BajoClaw 并完成连接，这篇讲全流程与连不上的排查。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与登录排查.md](常见问题与登录排查.md) · [联网搜索和网页阅读怎么用.md](联网搜索和网页阅读怎么用.md)

---

> [!IMPORTANT]
> **BajoSeek 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/4d229aa59804](https://pan.quark.cn/s/4d229aa59804)

---

## 一、这套东西是什么、装在哪

BajoClaw 是 BajoSeek 团队基于开源框架 OpenClaw 封装的电脑端程序。你在手机 BajoSeek 里用自然语言发任务（比如整理邮件、定时抓数据、盯一个指标），指令通过官方连接服务送到你电脑上的 BajoClaw 执行，结果回传到手机对话里。

设备分工：

| 设备 | 装什么 | 干什么 |
| --- | --- | --- |
| 手机 | BajoSeek App | 用对话下发任务、收结果 |
| 电脑（保持开机） | BajoClaw（Windows） | 实际执行任务 |

## 二、电脑端安装（Windows）

1. 打开官方仓库发布页：[github.com/bajoseek/bajo-claw/releases](https://github.com/bajoseek/bajo-claw/releases)，下载最新的 win-x64 安装包（写作时是 `bajo-claw-1.0.7-win-x64.exe`，以发布页当时显示为准）。
2. 双击安装包，按提示装完。运行环境已内置，不需要自己装 Node.js、Git 或 OpenClaw。
3. 系统要求：Windows 10 / 11，64 位。

## 三、两步配置

**第一步，配模型**——BajoClaw 执行任务时调用的模型，用你自己的模型服务密钥：

1. 打开 BajoClaw，进「模型」页，点「添加提供商」。
2. 填三项：基础 URL（OpenAI 兼容接口地址，一般以 `/v1` 结尾）、API Key（你的密钥）、模型 ID（例如 `glm-5`、`deepseek-v4`，以你实际开通的服务为准）。
3. 点「验证」，通过后保存。

**第二步，连 BajoSeek**：

1. 进「频道」页，填 BajoSeek Bot ID 与 Token / Bot Key（在 BajoSeek 应用内对应的 Bot 配置处获取）。
2. 连接地址默认是 `wss://ws.bajoseek.com`，正常不用改。
3. 保存后回到手机 BajoSeek，发一条简单指令（比如「看看电脑上现在的日期时间」）验证链路。

## 四、连不上、没反应，按这个查

| 现象 | 逐项检查 |
| --- | --- |
| 模型没响应 | 基础 URL 是否带 `/v1`；API Key 是否填对、还有没有额度；模型 ID 是否拼对 |
| BajoSeek 连不上 | Bot ID 与 Token 是否正确；应用内 Bot 后台是否已配置完成；电脑网络能否访问 `wss://ws.bajoseek.com` |
| 技能市场打不开 | 官方仓库给的备用方案：访问 `clawd.org.cn` 手动下载技能压缩包，解压到应用提示的技能目录 |
| 手机发指令没反应 | 确认电脑端 BajoClaw 已启动并保持运行；手机端重发一次指令 |

官方完整教程在仓库内：[docs/USAGE.zh-CN.md](https://github.com/bajoseek/bajo-claw/blob/main/docs/USAGE.zh-CN.md)。

## 五、用之前想清楚的三件事

- **版本**：BajoClaw 是较新的能力，网盘里的 1.5.0 未必带这个入口，手机应用里找不到就先更新到商店最新版，见 [下载与安装教程.md](下载与安装教程.md) 的获取渠道一节。
- **钥匙安全**：API Key 与 Bot Token 都是你自己的凭证，不要截图外发；只在你自己的电脑上配置。
- **边界**：让它执行的是你自己电脑上、你自己账号里的正当任务。给它的权限越大，下指令前越要想清楚这条指令会动哪些文件、哪些数据。
