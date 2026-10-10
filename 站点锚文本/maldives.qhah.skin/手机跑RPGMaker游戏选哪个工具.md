# 手机跑 RPG Maker 游戏选哪个工具

> RPG Maker 是个大家族，从 2000 到 MZ 出了很多代，安卓上没有一款工具能全包——本篇按引擎把对口工具理清楚，帮你一步到位找对方向。
> **相关文档**：[安卓怎么玩RPGMakerMV游戏.md](安卓怎么玩RPGMakerMV游戏.md) · [MV游戏手机上黑块或加载失败怎么办.md](MV游戏手机上黑块或加载失败怎么办.md)

## 一、为什么先看引擎再谈工具

RPG Maker 各代引擎的底层完全不同：老一代（2000 / 2003）是 RTP 素材体系，中间一代（XP / VX / VX Ace）是 Ruby 脚本，新一代（MV / MZ）则是 HTML5 + JavaScript，跑在浏览器内核上。引擎不同，运行器的实现思路就不同，所以「哪个工具最好」这个问题，答案永远取决于你手里游戏的引擎。

## 二、引擎与工具对照表

这是社区通行的分工，供直接对号入座：

| 手里的游戏引擎 | 对口的安卓工具 |
| --- | --- |
| RPG Maker MV / MZ | Maldives player（中文社区常叫 maldives 模拟器）、JoiPlay 配 RPG Maker 插件 |
| RPG Maker XP / VX / VX Ace | JoiPlay（配合 RPG Maker 插件） |
| RPG Maker 2000 / 2003 | EasyRPG Player |
| Ren'Py、TyranoBuilder 等 | JoiPlay |

MV/MZ 一行里两个工具都能跑，但社区口碑有差异：有游戏作者在 itch.io 的安卓安装说明里直接建议玩家用 Maldives Player，理由是 JoiPlay 读动画精灵图容易出错、大部分动画会崩；用 CanvasRendering 之类图像插件做渲染的 MV 游戏，在 JoiPlay 上也更容易报渲染错误。反过来，JoiPlay 的优势在引擎覆盖面广、社区资料积累多。

## 三、一个可直接执行的判断顺序

1. 按目录特征认引擎：有 `www` 子文件夹和 `index.html` 的是 MV/MZ；只有 `RPG_RT.exe` 的是老引擎；
2. 是 MV/MZ：先装 maldives 模拟器试，跑不顺再用 JoiPlay 对照——两者都免费，同时装互不冲突；
3. 是 XP / VX / VX Ace 或 Ren'Py：直接用 JoiPlay；
4. 是 RPG Maker 2000 / 2003：用 EasyRPG Player；
5. 换工具之前，先确认游戏完整解压、添加时目录选对——相当一部分「这工具不行」其实是文件本身的问题。

## 四、只想玩 MV/MZ 的话

专精路线更省心：maldives 模拟器这类只针对 MV/MZ 的运行器，把自动修插件错误、NPOT 黑块补丁、性能增强这些都做进了设置页，不用到处找教程配环境。它的界面实拍与排查思路可以看 [maldives 模拟器介绍站](https://maldives.qhah.skin/)，先确认界面和功能对得上自己的需求再装也不迟。
