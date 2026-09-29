# FeedMe 支持的 RSS 服务与账号登录

> FeedMe 本身不带文章源，它是个客户端；这篇讲它支持哪些 RSS 服务、各家的能力差异，以及怎么登录。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [同步方式与离线阅读.md](同步方式与离线阅读.md) · [常见问题排查.md](常见问题排查.md) · [自建RSS服务对接要点.md](自建RSS服务对接要点.md)

---

> [!IMPORTANT]
> **FeedMe 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/8eb12ca71bcc](https://pan.quark.cn/s/8eb12ca71bcc)

---

## 一、先用一句话理清关系

你要先有一个 RSS 服务（账号），再用 FeedMe 登录它：订阅哪些源、已读没读、加过星的文章，这些状态都存在服务端，FeedMe 负责拉取下来给你读。所以「选哪个服务」值得在装应用之前先想清楚，换服务基本等于换一个账号重来。

## 二、官方支持的服务清单

官方文档列出的支持范围：

- Feedbin
- Feedly
- Fever API —— 兼容 CommaFeed、Miniflux
- Folo
- Tiny Tiny RSS（常缩写 TTRSS）
- Google Reader API —— 兼容 Bazqux、FreshRSS、InoReader（Inoreader）、The Old Reader

其中 FreshRSS、Tiny Tiny RSS、CommaFeed、Miniflux 是可以自己部署的开源服务，其余是托管服务，注册账号就能用。

## 三、各服务能力对照

不同服务在客户端里的能力并不对等。官方文档给出的对照如下：

| 服务 | 在客户端订阅新源 | 建立/删除标签 | 加星 | 只抓取单个订阅源 |
| --- | --- | --- | --- | --- |
| Feedly | 支持 | 支持 | 支持 | 支持 |
| Google Reader API（Inoreader / Bazqux / The Old Reader / FreshRSS） | 支持 | 支持 | 支持 | 支持 |
| Feedbin | 支持 | 支持 | 支持 | 不支持 |
| Tiny Tiny RSS | 支持 | 不支持 | 支持 | 支持 |
| Fever API | 不支持 | 支持 | 支持 | 不支持 |
| Folo | 支持 | 不支持 | 支持 | 支持 |

这张表的两个实际影响：

- 用 Fever API（含 Miniflux、CommaFeed）时，FeedMe 里订阅不了新源——要先去服务端网页界面订阅，客户端只负责读；
- 标签在 Tiny Tiny RSS 和 Folo 上管理不了，习惯用标签整理文章的话，选型时要留意。

## 四、登录步骤

1. 打开 FeedMe，首次启动会进入服务选择页；
2. 按你的服务类型操作：
   - 托管服务（Feedly、Feedbin、Inoreader、Folo 等）→ 选择后跳网页授权，登录确认后回到应用；
   - 自建服务（FreshRSS、TTRSS、Miniflux 等）→ 填服务地址加账号密码，地址就是你部署服务的域名；
3. 登录成功后应用开始同步。第一次同步建议在 WiFi 下进行，原因见 [同步方式与离线阅读.md](同步方式与离线阅读.md)。

想换服务或换了版本找不到入口时，优先到 设置 里找服务 / 账号相关的条目，界面随版本有微调，以你手里版本的显示为准。

## 五、自建还是托管，怎么选

- 只想注册个号就用：Feedly、Inoreader、Feedbin 这类托管服务，注册即用；多数有免费档，够不够用要看你的订阅量和功能需求，以各家官网当时的说明为准；
- 订阅量大、在意数据归属、或手上本来就有服务器：FreshRSS、Tiny Tiny RSS、Miniflux 自建，FeedMe 对这三家的支持都比较完整（都走 Google Reader API 或 Fever API 兼容层）；服务端要开哪些开关、密码该怎么设，见 [自建RSS服务对接要点.md](自建RSS服务对接要点.md)；
- 拿不准就先托管后自建：客户端是同一个，换服务只是换个账号重新登录，迁移成本主要是把订阅源在新服务里重新订一遍。
