# JoiPlay 提示缺少 RTP：RPG Maker 素材包报错处理

> RPG Maker XP/VX/VX Ace 的游戏启动时报 `Failed to load: Graphics/...` 这类错误，多半是缺 RTP 共享素材包。本篇讲 RTP 是什么、从哪拿、怎么加进 JoiPlay。
> **相关文档**：[常见问题与解决.md](常见问题与解决.md) · [插件安装与引擎支持.md](插件安装与引擎支持.md) · [怎么添加游戏.md](怎么添加游戏.md)

---

> [!IMPORTANT]
> **JoiPlay 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/03ae944d080e](https://pan.quark.cn/s/03ae944d080e)

---

## 一、RTP 是什么，哪些游戏需要它

RTP（Runtime Package，运行时素材包）是 RPG Maker 官方发布的一套共享素材（图块、角色、音效等）。用 RPG Maker XP / VX / VX Ace 做游戏时，作者可以不把这些素材打进游戏里，而是假定玩家电脑上装过 RTP —— 结果就是：游戏在装了 RTP 的电脑上能跑，拷到手机上就报缺文件。

判断标准很直接：

- 报错形如 `Failed to load: Graphics/System/xxx` 或 `Failed to load: Audio/xxx` → 缺 RTP 的典型表现（也可能是游戏文件夹不完整，先重新解压一遍排除）。
- 游戏是 RPG Maker XP / VX / VX Ace 做的（文件夹里有 `Game.exe`，旁边是 `Data`、`Graphics` 等目录）→ 这类游戏最常出 RTP 问题。
- RPG Maker MV / MZ 的游戏（文件夹里有 `www` 目录）**不需要 RTP**，素材都打包在游戏内；它们的报错另有原因，见 [常见问题与解决.md](常见问题与解决.md)。

## 二、从哪拿 RTP

去 RPG Maker 的官方渠道下载对应版本的 RTP（RPG Maker XP RTP / VX RTP / VX Ace RTP）。需要哪个版本看游戏用的引擎，分不清就三个都备着，每个几百 MB。下载下来是 zip 压缩包，**不用自己解压**，JoiPlay 会处理。

## 三、把 RTP 加进 JoiPlay

1. 打开 JoiPlay，点右上角菜单。
2. 找到「添加 RTP」（Add RTP，部分版本叫「手动添加 RTP」）。
3. 选与游戏匹配的版本（XP / VX / VX Ace），在文件选择器里**直接选中下载好的 RTP 压缩包**。
4. 等导入完成，重新启动游戏。

另一个常见入口是「先启动游戏」：游戏报缺 RTP 的提示时，界面上会出现选择按钮，直接在弹出的选择器里找下载好的 RTP 包选中即可 —— 两种入口效果一样。

导入时报「无法解压运行时包」之类的错误，按这两条查：

- 选中的必须是 RTP 的 zip 文件本身，不是解压出来的文件夹，也不是别的压缩包。
- 重新下载 RTP 再试 —— 包没下完整也会报这个错。

## 四、加了 RTP 还是报错

- **看报错缺的路径类型**：仍是 `Graphics/`、`Audio/` 开头 → RTP 版本可能选错了，换对应版本重新导入。
- **清掉旧的 RTP 再加**：部分版本提供清除 / 管理 RTP 的入口，先清空再重新导入，排除旧包损坏。
- **游戏文件夹本身不完整**：重新解压一遍游戏，再对照 [怎么添加游戏.md](怎么添加游戏.md) 检查目录结构。
- **游戏加密过**：素材打包成 `Game.rgssad` / `Game.rgss2a` / `Game.rgss3a` 并做了特殊加密的游戏，报 `Failed to read script data` 的话，JoiPlay 跑不了它 —— 这不是 RTP 能解决的。

## 五、缺 RTP 和缺插件的差别

两者都会让游戏打不开，修法完全不同：插件是**装一个 APK**（对照表见 [插件安装与引擎支持.md](插件安装与引擎支持.md)），RTP 是**往 JoiPlay 里导入素材包**。提示找不到 RPG Maker Plugin 时去装插件；提示 `Failed to load` 缺文件时来加 RTP。排除完这两样还打不开的，去 [常见问题与解决.md](常见问题与解决.md) 按现象排查。
