# SVPcast 电脑串流配置

> 本篇讲把插帧工作交给电脑的 SVPcast 方案：电脑端怎么开、手机端怎么连、连不上和卡顿怎么排。
> **相关文档**：[插帧功能设置与性能调优.md](插帧功能设置与性能调优.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **SVPlayer 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a99508618b6c](https://pan.quark.cn/s/a99508618b6c)

---

## 一、什么时候需要这台电脑

手机带不动机内插帧（处理器低于[硬件门槛](下载与安装教程.md#六系统与硬件要求)、4K 片源、想用电脑端 RIFE 神经网络插帧）时，官方给的替代路线是：电脑跑 SVP 4，开启 SVPcast 扩展，把**已经插好帧**的视频流推到局域网里，SVPlayer 只负责收流播放。手机端几乎零算力，发热和耗电问题随之消失。

版本对应关系（以官方 [SVPcast wiki](https://svp-team.com/wiki/SVPlayer_with_SVPcast) 为准）：

- SVPlayer **1.4.0 起**支持播放 SVPcast 串流——本文网盘里的 1.6.3 满足；
- 1.6.x 起支持通过组播自动发现局域网里的 SVPcast 服务器，多台也能识别；
- 1.7.0 起新增「把当前正在播的任意视频丢给电脑插帧」的 cast 按钮，需要 SVPcast 1.5.0 以上配合——1.6.3 没有这个按钮，能播的是电脑上共享出来的视频。

## 二、电脑端（SVP 4）准备

1. 在电脑上安装 [SVP 4](https://www.svp-team.com/get/)（Windows / macOS / Linux），并安装（Windows）或启用（macOS）SVPcast 扩展。SVPcast 是桌面版 SVP 4 的付费扩展；
2. 打开 Web 界面并选定共享的视频文件夹：
   - SVP 菜单 → Streaming → Web UI → Enabled；
   - SVP 菜单 → Streaming → Web UI → Choose videos folder；
3. 验证服务起来了：SVP 菜单 → Streaming → Web UI → Open in web browser，浏览器里能看到就绪；
4. 调插帧档案：默认套用内置的「SVPcast streaming」档案。想给手机单独一套参数（换目标帧率、开 RIFE 等），复制一份档案，加条件「Video player = svplayer」，再随意加一个正分值让它压过默认档案。

## 三、手机端连接与使用

1. 打开 SVPlayer，文件浏览器的「Locations」（位置）里应出现 SVPcast 共享；像浏览本地文件夹一样浏览电脑上共享的视频，直接点播；
2. **每次操作后等约 5 秒**——视频流要重新起，5 秒不是卡死，别在这个间隙里反复点；
3. 切音轨、切字幕都会重启流；字幕是**烧进画面里**的，当前版本的串流字幕字体不可调；
4. 拖进度条同样会重启流，拖完耐心等；
5. 设置里可以开 HEVC（H.265）编码并按网络情况调码率。

## 四、连不上：找不到 SVPcast 共享

- 手机和电脑在**同一个局域网**里是最常见的前提，先确认；
- 手机浏览器手动打开 `https://www.svp-team.com/cast`，看它跳转到的 IP 是不是电脑的——跳错了说明发现机制没走通；
- 局域网里有多台跑 SVP 的电脑时，给每台起唯一名字：SVP 的 All settings 里改 `cast.server.ddns.name`，SVPlayer 的 `browser.cast.name` 填同一个名字。

## 五、串流卡顿

串流的瓶颈通常在电脑端和网络，按这个顺序查：

1. **电脑算力够不够**：RIFE 插 4K 对 PC 本身就是重负载，PC 自己都跑不满就别指望流出来是顺的；
2. **看缓存水位**：进度条或 OSD 统计里有这条流的缓存状态，暂停一会儿让缓存填满再放；
3. **硬件编码器**：SVP 菜单 → Streaming → Video encoder，确认选了正确的硬编码器；
4. **降码率**：编码和码率都在 Streaming 设置里，网络慢就往低调。

机内插帧的调法（不开电脑那条路）在[插帧功能设置与性能调优](插帧功能设置与性能调优.md)第三节。
