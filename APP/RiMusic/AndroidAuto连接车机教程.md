# RiMusic AndroidAuto连接车机教程

> 手机连上车机后在 Android Auto 里找不到 RiMusic？按下面的顺序做，官方 FAQ 的完整步骤都在这。
> **相关文档**：[常见问题与排查.md](常见问题与排查.md) · [下载与安装教程.md](下载与安装教程.md) · [离线缓存与歌曲下载.md](离线缓存与歌曲下载.md)

---

> [!IMPORTANT]
> **RiMusic 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/9822c0485bd7](https://pan.quark.cn/s/9822c0485bd7)

---

## 一、先确认三件事

连车机之前，这三项缺一个都会白试：

1. **RiMusic 侧开关已开**：主页右上角菜单 → Settings → Miscellaneous（其他）→ Android Auto 区域，确认开关启用。
2. **手机与车机用同一根线或无线连接正常**：系统设置里搜「Android Auto」，能进到它的设置页说明连接本身没问题。
3. **RiMusic 已装好并至少打开过一次**（见 [下载与安装教程.md](下载与安装教程.md)）——没运行过的应用不会出现在任何应用列表里。

## 二、手机侧：把「未知来源」打开（关键一步）

多数「列表里没有 RiMusic」的情况卡在这一步。官方 FAQ 的流程：

1. 系统设置里搜索 **Android Auto**，点进第一个结果。
2. 连点页面**底部的版本号 10 次**，开启开发者模式。
3. 右上角三点菜单 → **Developer Settings（开发者设置）**。
4. 勾选 **Unknown sources（未知来源）**——不勾这项，第三方应用不会出现在 Android Auto 的可选列表里。
5. 返回 Android Auto 主页 → **Customize launcher（自定义启动器）** → 在列表里勾上 RiMusic。

## 三、车机上怎么用

勾选后，车机屏的 Android Auto 应用列表里会出现 RiMusic，点开即用；播放控制用车机屏幕或方向盘按键操作，具体以你的车机界面显示为准。手机锁屏不影响车机上的播放。

## 四、列表里还是没有 RiMusic 时

按官方 FAQ 给的顺序排查：

1. 把手机从车上**断开**，关掉手机上所有 Android Auto 相关界面。
2. 重新进一次 Android Auto 设置，回看 Customize launcher 列表里 RiMusic 是否出现（断开重连后再看是官方点名的做法）。
3. 仍没有：回头核对第一节的三件事，尤其是 Miscellaneous 里的开关与 Unknown sources 是否都还在勾选状态——系统更新有时会重置开发者选项。
4. 全部确认后依旧异常，症状本身与设置无关的话，转 [常见问题与排查.md](常见问题与排查.md) 查其他已知问题（项目已停止维护，个别车机兼容问题没有官方修复）。
