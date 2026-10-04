# 自建 RSS 服务手机客户端

> 这篇讲自建 RSS 服务（FreshRSS、Tiny Tiny RSS、Miniflux）怎么配一个安卓客户端：各家要开哪些开关、密码填哪把、登录失败按什么顺序查。以 FeedMe 的对接方式为例。
> **相关文章**：[安卓RSS阅读器哪个好用.md](安卓RSS阅读器哪个好用.md) · [RSS离线阅读与全文缓存.md](RSS离线阅读与全文缓存.md)

## 一、自建服务缺的是一个顺手的客户端

FreshRSS、Tiny Tiny RSS、Miniflux 这类开源服务的网页界面胜在管理方便，但手机上天天开网页读文章并不舒服。它们都提供标准 API，配一个支持对应协议的客户端，体验就和托管服务没有差别了。

## 二、各家的对接要点

**FreshRSS（Google Reader API）**

1. 设置 → Authentication → 打开「允许 API 访问」；
2. 设置 → Profile → 单独填一把「API 密码」——它和网页登录密码是两回事，每个用户都要单独设；
3. 客户端里选 Google Reader API 这一类，地址填 FreshRSS 域名，密码填这把专用 API 密码。

**Miniflux（Fever API）**

1. 设置 → Integrations → 启用 Fever，并单独设一组 Fever 用户名和密码；
2. 客户端里选 Fever API，用这组专用凭证登录。

注意 Fever 通道的能力边界：不支持在客户端里订阅新源——先去服务端网页订好，客户端负责读；「保存文章」的行为是给文章加书签。

**Tiny Tiny RSS**

1. 登录网页版，进个人偏好设置，启用「API 访问」；
2. 客户端里选 Tiny Tiny RSS，填部署域名和账号密码。这条通道在客户端里管理不了标签，整理文章要去服务端网页。

## 三、登录失败的通用排查顺序

按这个顺序查，覆盖绝大多数情况：

1. 服务端的 API 开关开了没有（FreshRSS、TTRSS 有独立开关，Miniflux 在集成页）；
2. 密码填的是不是专用 API 密码，而不是网页登录密码；
3. 地址是不是 `https://` 且手机网络下可达——不少自建服务只在内网或特定设备上能访问；
4. 服务器是否拒绝 `%2F` 转义斜杠（FreshRSS 自带的检测链接能直接测出来）；
5. 反向代理或防火墙有没有把 API 路径拦掉。

## 四、客户端怎么挑

挑客户端先看它对这几家的兼容层全不全：Google Reader API 和 Fever API 两条线都覆盖的，后面换服务端也不用换应用。FeedMe 两条兼容层都支持，FreshRSS、Tiny Tiny RSS、Miniflux 都能接；各服务在客户端里的能力对照表（订阅、标签、加星的差异）整理在 [FeedMe 介绍站](https://feedme.hfdk.space/)，对接前对照一眼能少走弯路。
