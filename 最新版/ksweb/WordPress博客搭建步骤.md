# KSWEB 跑 WordPress：博客搭建步骤

> 本篇把「手机上装一套 WordPress 博客」拆成四步讲透，Typecho 同理；装完常见的坑与数据备份一并给出。
> **相关文档**：[手机搭建网站教程.md](手机搭建网站教程.md) · [phpMyAdmin与数据库管理.md](phpMyAdmin与数据库管理.md) · [外网访问与内网穿透.md](外网访问与内网穿透.md)

---

> [!IMPORTANT]
> **KSWEB 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e6a01d3e1a7d](https://pan.quark.cn/s/e6a01d3e1a7d)

---

## 一、开工前确认三件事

- KSWEB 已装好并能跑通默认页面——还没装的话，用这份 [KSWEB 安装文件资源（夸克网盘）](https://pan.quark.cn/s/e6a01d3e1a7d)，装完先过一遍 [手机搭建网站教程.md](手机搭建网站教程.md) 的五步；
- 一个 Web 服务器、PHP、MYSQL 三个服务都处于开启状态；
- 存储权限已授予，否则解压进 `htdocs` 的文件服务端读不到。

## 二、四步装好 WordPress

1. **下载程序**：去 WordPress 中文官网 [cn.wordpress.org](https://cn.wordpress.org/) 拿安装包 zip；
2. **放进站点目录**：解压后把文件放进手机存储的 `htdocs`。两种放法：直接平铺进 `htdocs`（访问地址不带子目录），或整体放进 `htdocs/wordpress`（访问地址多一层 `/wordpress`，一台手机跑多个站时更清爽）；
3. **建空数据库**：按 [phpMyAdmin与数据库管理.md](phpMyAdmin与数据库管理.md) 把 phpMyAdmin 装好、登进去，建一个空库并记下库名；
4. **跑安装向导**：手机浏览器打开 `http://localhost:端口`（端口以主机条目显示为准，各方教程里 Apache 见过 8000、lighttpd 见过 8080），按向导填数据库信息：主机 `127.0.0.1`、端口 `3306`、用户名 `root`、密码空（改过就填新的）、库名填第 3 步建的；再设站点标题与管理员账号，装完的后台入口在 `/wp-admin`。

## 三、Typecho：旧手机可以选它

步骤与 WordPress 完全同构：[typecho.org](http://typecho.org/) 下载 → 解压进 `htdocs` → 建空库 → 浏览器访问 `install.php` 走向导。它比 WordPress 轻得多，内存紧张的旧手机跑起来压力小，后台原生支持 Markdown 写作。

## 四、装完常见的四个坑

- **改过数据库密码后网站连不上库**：WordPress 的连接信息存在 `wp-config.php` 里，改密码后要同步改它（见 [phpMyAdmin与数据库管理.md](phpMyAdmin与数据库管理.md) 第三节）；
- **页面白屏或 500**：多半是 PHP 版本与程序不匹配，去 PHP 分栏切版本，排查思路见 [常见问题与故障排查.md](常见问题与故障排查.md)；
- **放一会儿服务就断**：手机省电机制杀后台导致的，处理办法见 [常见问题与故障排查.md](常见问题与故障排查.md) 第三节；
- **只在自己手机上看得到**：局域网之外的访问要走内网穿透，见 [外网访问与内网穿透.md](外网访问与内网穿透.md)。

## 五、数据备份

写了一段时间的文章都存在数据库里，定期在 phpMyAdmin 里「导出」成 SQL 文件留底；搬家（换手机、换设备）靠的也是同一对操作：旧库导出、新库导入。操作位置见 [phpMyAdmin与数据库管理.md](phpMyAdmin与数据库管理.md) 第四节。
