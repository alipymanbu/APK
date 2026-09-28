# Maldives player 和 JoiPlay 怎么选

> 两个都是安卓上跑 RPG Maker 游戏的运行器，常被放在一起比较。本篇按引擎范围和社区反馈的体验差异，给你一个能直接执行的判断顺序。
> **相关文档**：[支持哪些游戏格式.md](支持哪些游戏格式.md) · [怎么添加游戏.md](怎么添加游戏.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **Maldives player 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/12bf8da36a2d](https://pan.quark.cn/s/12bf8da36a2d)

---

## 一、先看引擎，再谈选择

两者覆盖的引擎范围不同，多数情况下不是二选一的关系：

| 你的游戏是 | 能跑的运行器 |
| --- | --- |
| RPG Maker MV / MZ | Maldives player、JoiPlay（配合 RPG Maker 插件） |
| RPG Maker XP / VX / VX Ace | JoiPlay（Maldives player 不支持） |
| RPG Maker 2000 / 2003 | EasyRPG Player（两者都不负责） |
| Ren'Py、TyranoBuilder 等其他引擎 | JoiPlay（Maldives player 不支持） |

所以第一个判断永远是游戏引擎，范围对不上就谈不上比较。

## 二、同为 MV/MZ 时，社区反馈的差异

口径先说明：以下来自用户评论与第三方指南（截至 2026-09 记录），两边都在快速迭代，参考即可。

- **兼容性口碑**：不少用户把 Maldives player 当 JoiPlay 的替代品，评价集中在「MV/MZ 跑得更顺、设置省事」；有游戏作者在 itch.io 评论区直接建议玩家用 Maldives player，理由是 JoiPlay 读动画精灵图会出错、大部分动画会崩。
- **渲染类插件**：有第三方指南提到，用图像处理类插件（如 CanvasRendering）做渲染的 MV 游戏，在 JoiPlay 上容易报渲染错误，在 Maldives player 上通常能正常跑。
- **性能与操作**：中文社区对 Maldives（常写作 MaldiVes）的评价是比 JoiPlay 更精简、性能更好，JoiPlay 上被诟病的手柄映射问题在它这里不存在。
- **反面反馈也要知道**：Maldives player 自己的评论区里，加载失败、开增强功能后崩溃的反馈同样不少（排查思路见 [常见问题排查.md](常见问题排查.md)）；JoiPlay 胜在引擎覆盖广、社区资料积累多。

## 三、一个可执行的顺序

1. 先按 [支持哪些游戏格式.md](支持哪些游戏格式.md) 确认游戏引擎；
2. 是 MV/MZ：先装 Maldives player 试，跑不起来再用 JoiPlay 对照——两者都免费，同时装互不冲突；
3. 是 XP/VX/VX Ace 或 Ren'Py 等：直接用 JoiPlay；
4. 是 RPG Maker 2000/2003：用 EasyRPG Player；
5. 换运行器之前，先确认游戏文件完整解压、目录选对——相当一部分「这个运行器不行」其实是文件本身的问题（见 [怎么添加游戏.md](怎么添加游戏.md)）。
