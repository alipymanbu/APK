# RetroAchievements 成就系统设置

> 本篇讲给经典游戏加「成就」的 RetroAchievements 服务怎么在手机上开启：账号注册、RetroArch 里的设置路径、硬核模式的限制，以及成就不弹、登录失效的处理。
> **相关文档**：[核心下载与选择.md](核心下载与选择.md) · [手柄与触屏按键设置.md](手柄与触屏按键设置.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **RetroArch 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/b4258d0ad915](https://pan.quark.cn/s/b4258d0ad915)

---

## 一、这是什么服务

RetroAchievements（[retroachievements.org](https://retroachievements.org/)）是社区维护的第三方成就平台：爱好者为一张张老游戏编写成就清单，你在模拟器里达成条件就能解锁，体验类似现代主机的奖杯。RetroArch 内置了对它的支持，但**该服务本身不由 RetroArch/Libretro 团队运营**，需要单独注册账号。它不分发游戏，成就只在你的游戏运行过程中做校验。

## 二、开启步骤

1. 到 [retroachievements.org](https://retroachievements.org/) 注册账号，并按邮件确认。
2. 打开 RetroArch，进 **Settings → Achievements**，打开成就开关，填入该站的用户名与密码。
3. 启动一个游戏，唤出快速菜单 → **Achievements**：能列出这个游戏的成就清单，说明已经连上。

成就需要联网实时校验；离线时达成的成就会缓存，但**只对当前这次会话有效**——退出前记得恢复联网。

## 三、硬核模式值不值得开

**硬核模式（Hardcore Mode）**开启后，即时存档、慢放、快进、金手指全部禁用——等于回到当年的实体机条件。回报是：解锁的成就计入站点的正式排行榜，还能参加社区活动。

| 模式 | 可用辅助 | 成就计入 |
| --- | --- | --- |
| 普通模式 | 即时存档、快进、金手指都可用 | 记录，但不进硬核排行 |
| 硬核模式 | 全部禁用 | 正式排行榜与社区活动 |

两个模式可以随时切换。注意它和即时存档的冲突：硬核模式下打 BOSS 前先存一档这条路被堵死，进度只能靠游戏内存档，操作相关设置见 [手柄与触屏按键设置.md](手柄与触屏按键设置.md)。

## 四、常见问题

- **成就做了条件却不弹**：先确认当次会话联网；还不出，检查你用的核心是否在 [RetroAchievements 官方支持列表](https://docs.retroachievements.org/general/emulator-support-and-issues.html)里——不在名单的核心成就逻辑常有出入，换成受支持的核心（核心选择见 [核心下载与选择.md](核心下载与选择.md)）。
- **突然登录失败**：多为登录令牌过期。到 **Settings → User → Accounts → RetroAchievements** 清掉账号信息重新填一遍即可。
- **成就进度哪里看**：登录 retroachievements.org 的账号页，能看到每个游戏的解锁进度，硬核解锁的奖杯有专属标记。

其他使用故障（黑屏、扫描、存档）与成就无关的，去 [常见问题排查.md](常见问题排查.md) 按图索骥。
