# ExaGear ET版和ED版有什么区别

> 网上的 ExaGear 分好几个变体，包名、游戏目录、数据包路径各不相同——数据放不进去、游戏找不到，多半是版本没对上。本篇帮对号。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [游戏添加与启动教程.md](游戏添加与启动教程.md) · [常见问题与解决办法.md](常见问题与解决办法.md)

---

> [!IMPORTANT]
> **ExaGear 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/561282c37c32](https://pan.quark.cn/s/561282c37c32)

---

## 一、先分清三个常见变体

ExaGear 是一个系列，Eltechs 当年按用途出了好几个应用，社区后来流通的包也沿用了这些底子。对号入座：

| 变体 | 包名 | 游戏放哪 | 数据包（OBB）放哪 |
| --- | --- | --- | --- |
| ET 版 | `com.eltechs.et` | `内部存储/ExaGear/` | 两种口径都有：见第二节 |
| ED 版（Windows Emulator） | `com.eltechs.ed` | `内部存储/Download/` | `内部存储/Android/obb/com.eltechs.ed/` |
| ES 版（Strategies） | `com.eltechs.es` | `内部存储/ExaGear/` | `内部存储/Android/obb/com.eltechs.es/` |

配套的官方版本号也各走各的（社区帖口径，以当时页面为准）：Strategies 停在 3.5.0、RPG 停在 2.6.8、Windows Emulator 停在 3.0.1。**版本号相同不代表同一个应用**——比如本套文档对应的这份就是「ET 版 3.5.0」，和「Strategies 3.5.0」不是一回事。

## 二、数据包路径：两种口径怎么办

关于 ET 版的 OBB 放哪，公开教程分成两派：

- 一派按**包名**放：`内部存储/Android/obb/com.eltechs.et/`；
- 另一派针对**带 `main.xx.com.eltechs.ed.obb` 命名的数据包**，按 `com.eltechs.ed` 放，多篇针对同一份安装包的教程都这么写。

两边并不矛盾——关键看**你手上数据包文件叫什么**：`main.xx.com.eltechs.ed.obb` 就放进 `com.eltechs.ed/`，名字带 `.et.` 的就放进 `com.eltechs.et/`。仍不确定就两个文件夹都建上、各放一份（路径互不冲突），首启能正常解包的那个就是对的。

## 三、怎么确认自己装的是哪个

1. 手机「设置 → 应用 → ExaGear → 应用信息」里看**包名**，直接对上表。
2. 或者进模拟器后看哪个目录里能看到你的游戏：`ExaGear` 文件夹有货 = ET/ES 这路；只有 `Download` 有货 = ED 这路。
3. 装的是本套文档这份（版本 3.5.0、大小 63.54M），那它就是 ET 版——事实核对见 [能玩哪些游戏与配置要求.md](能玩哪些游戏与配置要求.md) 第四节。

## 四、版本对上了，接下来

游戏目录、数据包、`bass.so` 三处位置确认无误后，按 [下载与安装教程.md](下载与安装教程.md) 走完首启；游戏放错文件夹、双击没反应这类症状与版本的对应关系，见 [常见问题与解决办法.md](常见问题与解决办法.md) 第一节。
