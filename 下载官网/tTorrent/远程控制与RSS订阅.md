# tTorrent 远程控制与RSS订阅

> 讲 tTorrent 的 Web 界面与 Transdroid / Transdrone 远程管理，以及用 RSS 订阅让新种子自动进下载队列。
> **相关文档**：[使用入门与首次设置.md](使用入门与首次设置.md) · [下载速度慢怎么办.md](下载速度慢怎么办.md) · [常见问题与解决办法.md](常见问题与解决办法.md)

---

> [!IMPORTANT]
> **tTorrent 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f377a105db72](https://pan.quark.cn/s/f377a105db72)

---

## 一、Web 界面：从浏览器管任务

tTorrent 内置 Web 界面（官方功能列表写作 Web interface）。开启与访问：

1. 在 tTorrent 的设置里勾选 **Web interface**；
2. 默认端口是 **1080**，可以改成别的（以你设置页显示的为准）；
3. 在同一局域网的浏览器里访问 `http://<设备IP>:1080/`（设备 IP 在手机的 WLAN 详情里能看到）；
4. 建议在同一设置页开启 **Authentication** 并设置用户名密码 —— 否则局域网里任何人都能操作你的下载任务；需要走公网访问时可开 **SSL**（HTTPS）。

Web 界面是自适应页面，手机 / 平板浏览器打开也能用；除了查看与控制现有任务，还能从种子文件、URL 或磁力链接添加新任务。页面上也可以开启 UPnP，让路由器自动为这个 Web 端口做映射。

## 二、Transdroid / Transdrone 怎么配合

官方标注 Web 界面兼容 **Transdroid / Transdrone**（安卓上的远程管理工具）。典型用法：一台设备当下载机放在家里，用另一台手机远程添加或暂停任务。

1. 在手机上装 Transdroid 或 Transdrone；
2. 设置 → Add server → Add normal, custom server；
3. **Server type 选 tTorrent**，填下载机的 IP、Web 界面端口（默认 1080）和第 1 节设置的用户名密码；
4. 保存后返回主界面，能看到远程任务列表即成功。

只在局域网内用不需要动路由器；想在外网控制，就要在路由器上把 Web 端口转发到下载机，并把第 1 节的认证和 SSL 开上 —— 不带认证的 Web 端口暴露到公网风险很大。

## 三、RSS：让新种子自动进队列

官方功能列表里写作 RSS support（automatically download torrents published in feeds）：

- 把资源发布页的 RSS 源地址加进订阅（RSS 设置在应用设置里）；
- 源里更新出新种子时，tTorrent 自动加入下载；
- 配合标签（Label）可以把不同 RSS 来源的文件落到各自指定的保存路径。

除 RSS 外还有一个自动加任务的口子：目录设置里的 **Watch incoming directory** —— 指定一个被监视的文件夹，往里面放 `.torrent` 文件就会自动创建任务。配合 FTP / 局域网共享往这个目录投文件，就是一套简易的远程投喂方案。

这套组合适合的场景：仅 WiFi 模式 + 家里路由器挂机下载，白天远程看进度；或 RSS 订阅 + 标签分目录追更，新内容自动落位。挂机时熄屏会停的话，见[锁屏后下载停止怎么办.md](锁屏后下载停止怎么办.md)。
