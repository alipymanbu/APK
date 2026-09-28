# 把 Sweezy Cursors 光标换进 Roblox 的步骤

> 官网为 Roblox 单独出了 PNG 光标文件，尺寸格式已按 Roblox 要求做好；本篇按官方教程讲怎么替换进客户端、怎么还原。
> **相关文档**：[Windows鼠标指针替换方法.md](Windows鼠标指针替换方法.md) · [光标库与主题分类盘点.md](光标库与主题分类盘点.md) · [光标不生效与常见问题排查.md](光标不生效与常见问题排查.md)

---

> [!IMPORTANT]
> **Sweezy Cursors 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/98d0dc49ea9b](https://pan.quark.cn/s/98d0dc49ea9b)

---

## 一、Roblox 光标和浏览器光标是两套文件

官网把用法分成三条线：浏览器里靠扩展的「Add Cursor」收藏应用；Windows 系统用 `.cur` / `.ani` 文件（见 [Windows鼠标指针替换方法.md](Windows鼠标指针替换方法.md)）；Roblox 用单独导出的 PNG 文件。本篇只讲第三条。

每个光标的详情页里，`Roblox Cursor.png` 与 `Roblox Pointer.png` 就是要用的两个文件，下载后不需要再裁剪或缩放。

## 二、替换步骤（官方教程口径）

1. 在官网挑好光标，从详情页下载 `Roblox Cursor.png` 与 `Roblox Pointer.png`；
2. 右键桌面上的 Roblox 快捷方式，选「打开文件所在位置」，依次进入 `content\textures\cursors\keyboardmouse` 文件夹；
3. 先备份原文件：把文件夹里的 `ArrowCursor.png` 与 `ArrowFarCursor.png` 改名留档（比如在文件名后面加 `.bak`）；
4. 把下载的两个文件改名为 `ArrowCursor.png` 与 `ArrowFarCursor.png`，放进这个文件夹；
5. 完全退出 Roblox 再重新打开，新光标生效。

## 三、想换回去

把第二步备份的两个原文件改回原名、放回原文件夹覆盖替换文件，重启 Roblox 即可还原。哪天光标自己变回了默认样子，多半是文件被覆盖了，按第二节再放一次就行。

## 四、动手前想清楚的三件事

- 整个过程只动两张图片文件，不碰游戏本体；原文件备份是唯一的退路，别省这一步；
- Roblox 官方并没有提供自定义光标支持，这是玩家社区通行的图片替换做法；有第三方文章认为纯装饰性的指针替换不涉及游戏机制与不公平优势，但最终是否替换由你自己判断；
- 文件只从官网光标详情页下载，别的渠道来的同名文件先放一放。
