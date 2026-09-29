# 自建 RSS 服务对接要点

> 这篇讲怎么把 FreshRSS、Miniflux、Tiny Tiny RSS 这类自建服务接进客户端：服务端要开哪些开关、密码填哪个、登录失败按什么顺序查。
> **相关文档**：[RSS服务接入与账号登录.md](RSS服务接入与账号登录.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **FeedMe 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/8eb12ca71bcc](https://pan.quark.cn/s/8eb12ca71bcc)

---

## 一、为什么单独写这篇

自建服务登录失败，绝大多数不是客户端的问题，而是两件事：服务端的 API 没开，或者把网页登录密码当成了 API 密码填。各家服务端对此的要求不同，先在服务端做完准备，再去客户端登录，一次就通的概率最高。

## 二、FreshRSS：两个开关加一把专用密码

服务端要做的（以 FreshRSS 官方文档为准）：

1. 设置 → Authentication（认证）→ 打开「Allow API access（允许 API 访问）」；
2. 设置 → Profile（个人资料）→ 填「API password（API 密码）」——**每个用户都要单独设**，这把密码和网页登录密码是两回事，专门给手机应用用，泄露了也不波及网页账号。

填完后用官方自带的测试：Profile 页 API 密码旁边有一个形如 `https://rss.example.net/api/` 的链接，点开按提示检测，返回 PASS 就绪。

两个常见报错对应的服务端原因：

| 报错 | 原因与处理 |
| --- | --- |
| Bad Request / Not Found | 服务器不接受被转义成 `%2F` 的斜杠，Apache 加 `AllowEncodedSlashes On` |
| FAIL getallheaders | PHP 与 Web 服务器组合取不到请求头，开 `mod_setenvif` 或 `mod_rewrite` |

客户端侧：在 [RSS服务接入与账号登录.md](RSS服务接入与账号登录.md) 里选 Google Reader API 这一类，地址填你的 FreshRSS 域名，账号填用户名，密码填 **API 密码**。

## 三、Miniflux：走 Fever API，凭证在集成页单独设

- Miniflux 官方文档把 FeedMe 列为 Fever API 的兼容应用，对接走 Fever 这条路；
- 服务端：设置 → Integrations（集成）→ 启用 Fever，并**单独设一组 Fever 用户名和密码**——不是你的 Miniflux 登录密码；
- 客户端：FeedMe 里选 Fever API，地址填 Miniflux 实例域名，用上面那组专用凭证登录。

Miniflux 官方文档还注明了几条 Fever 通道的能力边界：只支持 JSON 格式；不支持 sparks、kindlings 这类分组概念；在客户端里「保存文章」的行为是给该文章加书签。知道这些，就不会把「功能怎么少一截」当成故障。

## 四、Tiny Tiny RSS：先在用户偏好里启用 API

1. 登录 TTRSS 网页版，进个人偏好设置，启用「API 访问」（入口名称随版本略有差异）；
2. 客户端里选 Tiny Tiny RSS，地址填部署域名，账号密码即 TTRSS 的用户凭证。

提醒一点：按官方能力对照，TTRSS 通道在客户端里管理不了标签，整理文章要在服务端网页上做。

## 五、CommaFeed：同样走 Fever API

CommaFeed 属于 Fever API 兼容名单里的服务，服务端部署好后在 FeedMe 里选 Fever API，填部署地址登录。能订阅新源这件事要在服务端网页完成（Fever 通道不支持客户端订阅，见 [RSS服务接入与账号登录.md](RSS服务接入与账号登录.md) 的能力对照表）。

## 六、登录失败的通用排查顺序

按这个顺序查，覆盖了绝大多数情况：

1. 服务端的 API 访问开关开了没有（FreshRSS、TTRSS 都有独立开关，Miniflux 在集成页启用）；
2. 密码填的是不是**专用 API 密码**，而不是网页登录密码（FreshRSS 和 Miniflux 都要求单独设置）；
3. 地址是不是 `https://` 且手机网络下能访问——很多自建服务只在内网或特定设备上可达；
4. 服务器是否拒绝 `%2F` 转义斜杠（FreshRSS 的检测链接能直接测出来）；
5. 前面的反向代理或防火墙有没有把 API 路径拦掉。

以上各家界面的名称以你部署版本的服务端文档为准，这类设置很少变动，但报错文案会随版本不同。
