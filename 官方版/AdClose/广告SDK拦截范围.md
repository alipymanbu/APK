# AdClose 支持的广告 SDK 与适用应用

> 本篇列出官方说明里 AdClose 能处理的广告 SDK 清单和适用的应用类型，帮你判断「我常用的这个应用，它管不管用」。
> **相关文档**：[功能与作用域配置.md](功能与作用域配置.md) · [下载与安装教程.md](下载与安装教程.md) · [版本与更新日志.md](版本与更新日志.md)

---

> [!IMPORTANT]
> **AdClose 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e3caea48d15a](https://pan.quark.cn/s/e3caea48d15a)

---

## 一、覆盖从哪看起

模块对广告的处理走两条路：一是阻止应用内广告 SDK 完成初始化，二是拦截应用发出的广告网络请求。一个应用里的广告通常同时踩这两条路，所以功能一二两项默认都该开（各项开关的设置位置见 [功能与作用域配置.md](功能与作用域配置.md)）。

判断「管不管用」的顺序：先看下面第二节的应用类型，再对照第三节的具体 SDK 清单。都符合、开关也开了，但广告还在，就属于能力边界问题，见第四节。

## 二、适用的应用类型

官方说明支持众多流行应用，包括但不限于：

- 视频播放应用
- 阅读和新闻应用
- 工具和便捷应用
- 商务办公应用
- 社交和通讯应用

这个分类是按「应用里常出现哪种广告」归纳的。开屏广告、横幅、弹窗、信息流这几类是主要拦截对象。

## 三、SDK 清单

以下为官方仓库说明中列出的、模块能够处理的 SDK 类型（按官方原文收录，不做增删）：

| 类别 | SDK |
| --- | --- |
| 字节系 | ByteDance (Pangolin) Ads（穿山甲）、ByteDance (Pangolin) GroMore |
| 腾讯系 | Tencent Ads、Tencent SDK |
| Google 系 | Google Ads、Google FireBase Crashlytics Sdk、FaceBook Ads |
| 国内大厂 | Baidu Ads、Huawei Ads、Xiaomi Ads、Kwai Ads、Ali BaiChuan Ads、Pinduoduo Ads |
| 海外平台 | Applovin Sdk、Inmobi Ads、Mintegral Ads、Mbridge Ads、Unity3d Ads、Vungle Sdk |
| 聚合与其他国内平台 | ADSuyiSdk Ads、AdScope、Appic Ads、BJXingu Ads、MeiShu Ads、Qumeng Ads、Sigmob Ads、Tanx Ads、TopOn SDK (anythink)、TradPlus Ads、XiaoChuang Ads (ZuiYou)、XinwuPaijin Ads |
| 统计类 | Umeng SDK |

「类别」一栏是按常见认知做的分组，方便你快速找；以 SDK 名称为准。清单会随版本更新，以官方仓库当前说明为准。

## 四、能力边界

- **清单里的 SDK，不代表每个应用里的所有广告位都能拦干净**。同一 SDK 在不同应用里的接入方式不同，实际效果以你逐个应用试出来的为准。
- **清单之外的广告来源**，模块默认处理不了。想针对特定应用追加拦截，走规则与自定义途径，见 [规则引入与自定义拦截.md](规则引入与自定义拦截.md)（还没配好开关的，先看 [功能与作用域配置.md](功能与作用域配置.md)）。
- **效果和版本有关**。广告形式在变，模块也在跟着更新；官方仓库曾专门优化过自动检测广告 SDK 的方式（4.3.2）。本地这份安装包是 4.2.2，版本差异见 [版本与更新日志.md](版本与更新日志.md)。
