# APatch 资料站

围绕安卓内核级 Root 方案 APatch 的第三方资料区：介绍它的工作原理、机型门槛，以及刷入过程中的关键环节（超级密钥、模块体系、Zygisk 环境）。

想快速了解这个方案，可以从 [APatch 介绍与刷入指引](https://apatch.djnf.quest/) 看起，那里有两页完整的内容：方案原理页与安卓版下载安装操作页。

## 本目录文章

- [内核级Root方案怎么选](内核级Root方案怎么选.md) —— APatch、Magisk、KernelSU 三套方案的分界线与选型思路
- [超级密钥设置与保管](超级密钥设置与保管.md) —— 唯一解锁凭据的设定规则与遗忘后果
- [Zygisk环境怎么搭](Zygisk环境怎么搭.md) —— 默认不带 Zygisk 的 APatch 如何加载依赖它的模块
