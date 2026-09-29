# Chub AI 角色卡导出与SillyTavern互通

> 本篇讲怎么把 Chub 的角色卡与聊天记录带去 SillyTavern 等其他工具：卡的两种格式、进出两个方向的接法，以及把 Chub 的模型接进第三方前端。
> **相关文档**：[角色查找与聊天入门.md](角色查找与聊天入门.md) · [自建角色卡教程.md](自建角色卡教程.md) · [API连接与模型配置教程.md](API连接与模型配置教程.md)

---

> [!IMPORTANT]
> **Chub AI 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/9c82a8ab7b35](https://pan.quark.cn/s/9c82a8ab7b35)

---

## 一、卡的两种格式

| 格式 | 是什么 | 适用 |
| --- | --- | --- |
| PNG 卡 | 内嵌了设定数据的图片，头像与设定一体 | 最通用的形态，优先选它 |
| JSON 卡 | 纯数据文件，不含图片 | 只要设定不要头像时用 |

角色详情页的下载按钮里两种都能拿到。

## 二、从 Chub 导入 SillyTavern

1. 在 Chub 角色页下载 PNG 或 JSON 文件；
2. 打开 SillyTavern 的角色管理面板，点 Import Character 选择文件，或直接把 PNG 拖进 SillyTavern 窗口；
3. 导入后头像与设定自动就位，选中即可开聊。

导入没反应先查两件事：文件是否完整下载（重新下一次）、SillyTavern 版本是否过旧（更新后再试）。

## 三、从别的工具搬进 Chub

- 酒馆 PNG 卡可以直接导入：创建器或角色创建页的头像字段处支持导入 Tavern PNG，设定随卡读入，流程见 [自建角色卡教程.md](自建角色卡教程.md)；
- 从 Character.AI 迁移的卡，先用 ZoltanAI Character Editor（`https://zoltanai.github.io/character-editor/`）转成标准角色卡格式再导入。

## 四、聊天记录的进出

- **导出**：聊天页 Chat Settings 里可把当前对话导出为 JSONL（SillyTavern 格式）、PNG 或纯文本，带去 SillyTavern 继续聊或做存档；
- **导入**：官方文档提到支持从既有服务导入聊天，入口与支持范围以官方文档当时显示为准。

## 五、把 Chub 的模型接进别的前端

订阅完整档（Mars）后，模型不只能用在 Chub 自己的 App 与网页里，还能以「OpenAI 接口模拟」的形式接进其他聊天前端使用；官方文档设有专门章节（`docs.chub.ai` 的 Usage with Third-Party UIs）。接口地址与配置项以官方文档当时显示为准，各 API 与订阅档位的差别见 [API连接与模型配置教程.md](API连接与模型配置教程.md)。

反过来，SillyTavern 这类前端的聊天记录与卡搬进搬出时，最容易丢的是世界书条目与备选开场白这类扩展字段，搬完先开一段新对话核对一遍。
