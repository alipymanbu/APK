# astrbot 接入哪家大模型

> AstrBot 安卓 App 装完只是有了机器人身体，填一把大模型 API 密钥它才会说话。本篇按「你手里有什么」给接法，顺带说清各家密钥从哪申请。
> 相关文章：[astrbot 手机上怎么部署](astrbot手机上怎么部署.md) · [astrbot 初始化失败怎么办](astrbot初始化失败怎么办.md)

## 一、按你手里有什么选

框架兼容 OpenAI、Google、Anthropic 三种原生接口，外加一切 OpenAI 兼容的第三方服务：

| 你的情况 | 可选的接法 |
| --- | --- |
| 有海外大平台账号 | OpenAI（GPT 系列）、Google Gemini、Anthropic（Claude）原生接口 |
| 想用国产模型 | DeepSeek、智谱 GLM、Kimi、MiniMax 等，多走 OpenAI 兼容接口 |
| 不想花 API 钱 | Ollama、LM Studio 里跑开源模型，填本机或局域网地址 |
| 有现成的智能体应用 | Dify 等应用可直接接入 |

## 二、密钥从哪申请（申请入口经核对，2026-09）

| 平台 | 入口 | 备注 |
| --- | --- | --- |
| DeepSeek | platform.deepseek.com | OpenAI 兼容，base_url 为 api.deepseek.com |
| 硅基流动 | siliconflow.cn | 聚合多家开源模型，一把 Key 调多家 |
| Ollama / LM Studio | 不需要 Key | 跑在自己设备上，填局域网地址即可 |
| OpenAI / Gemini / Claude | 各自开放平台 | 需要相应账号与网络条件 |

各家价格与免费额度随时会变，接入前以对应平台当时的页面为准。

## 三、填进去的步骤

1. 管理面板进「模型提供商」（Provider）页；
2. 新增提供商，选你手里的服务类型；
3. 粘贴 API Key，选一个默认模型，保存；
4. 按提示重启机器人，配置才生效。

装好后别忘了验证：QQ 里发条消息看它回不回；发 `/provider`（管理员）可以随时切换提供商。

## 四、装不上或初始化没过，先别急着买 Key

机器人本体没跑起来之前，密钥填了也没用。先按 [astrbot 手机上怎么部署](astrbot手机上怎么部署.md) 把装机五环节走完，卡住对照 [astrbot 初始化失败怎么办](astrbot初始化失败怎么办.md) 排查；App 的功能边界（支持哪些平台、哪些能力没有）在 [astrbot 安卓 App 介绍站](https://astrbot.yoan.quest/) 首页有完整对照表。
