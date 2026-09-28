# tTorrent 远程控制与RSS订阅

> 讲 tTorrent 的 Web 界面与 Transdroid / Transdrone 远程管理，以及用 RSS 订阅让新种子自动进下载队列。
> **相关文档**：[使用入门与首次设置.md](使用入门与首次设置.md) · [下载速度慢怎么办.md](下载速度慢怎么办.md) · [常见问题与解决办法.md](常见问题与解决办法.md)

---

> [!IMPORTANT]
> **tTorrent 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f377a105db72](https://pan.quark.cn/s/f377a105db72)

---

## 一、Web 界面：从浏览器管任务

tTorrent 内置 Web 界面（官方功能列表写作 Web interface）。开启后你可以：

- 在同一局域网的电脑浏览器里查看与控制手机上的下载任务；
- 配合支持该界面的安卓远程管理工具使用，官方标注兼容 **Transdroid / Transdrone**。

开启方式在应用设置里，端口与访问地址以你设备上显示的为准。

## 二、Transdroid / Transdrone 怎么配合

典型用法：一台手机当下载机放在家里，用另一台设备远程添加或暂停任务。装好 Transdroid / Transdrone 后按它的引导添加 tTorrent 服务端即可，两者通信走的就是上面那个 Web 界面。

## 三、RSS：让新种子自动进队列

官方功能列表里写作 RSS support（automatically download torrents published in feeds）：

- 把资源发布页的 RSS 源地址加进订阅；
- 源里更新出新种子时，tTorrent 自动加入下载；
- 配合标签（Label）可以把不同 RSS 来源的文件落到各自指定的保存路径。

这套组合适合的场景：仅 WiFi 模式 + 家里路由器挂机下载，白天远程看进度；或 RSS 订阅 + 标签分目录追更，新内容自动落位。
