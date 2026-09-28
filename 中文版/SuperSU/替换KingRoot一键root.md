# 替换 KingRoot 一键 root 的做法

> 本篇讲用 KingRoot/KingUser 这类一键工具 root 过的手机，怎么换成 SuperSU 来管权限：最稳的干净重刷路线、免重刷的就地接管路线，以及不建议碰的脚本法。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [完整卸载与取消root方法.md](完整卸载与取消root方法.md) · [常见问题与报错处理.md](常见问题与报错处理.md)

---

> [!IMPORTANT]
> **SuperSU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ae57de1aba73](https://pan.quark.cn/s/ae57de1aba73)

---

## 一、为什么建议换，两条路线怎么选

KingRoot 一键 root 之后留在系统里的管理器是 KingUser，它和 SuperSU 干同一件事。想换的常见动机：KingUser 更新跟不上、授权行为不够透明、部分 root 工具兼容不好。

换个管理器不是"装个新应用"那么简单——**su 二进制同时只能有一套**，两个管理器并存会互相抢。两条路线按情况选：

| 路线 | 做法 | 适合 |
| --- | --- | --- |
| A 干净重刷 | 先用 KingUser 取消 root，再用 TWRP 重新刷 SuperSU | 手机上有 TWRP 的，最稳，推荐 |
| B 就地接管 | 保留现有 root，装 SuperSU 让它接管 su，最后删 KingUser | 不想再进 recovery 的，一般也够用 |

## 二、路线 A：先取消 root，再重刷（推荐）

1. 打开 KingUser（KingRoot 系的管理器），进设置，执行「完全删除 Root / Remove Root permission」，按提示重启；
2. 重启后用 root 检测工具确认 root 已清掉；
3. TWRP 还装在手机上，直接进 recovery，按 [下载与安装教程.md](下载与安装教程.md) 第四节把 ZIP 重新刷一遍；
4. 刷完装 APK、更新 SU 二进制，流程与第一次 root 完全一样。

这条路全程只有一个管理器在场，不存在"谁接管谁"的问题；代价是多进一次 recovery。

## 三、路线 B：SuperSU 就地接管（免重刷）

多个社区教程给出的口径一致，按这个顺序来：

1. 从开头的 [SuperSU 安装文件资源（夸克网盘）](https://pan.quark.cn/s/ae57de1aba73) 拿 APK 装进手机（此时**先别动 KingUser**）；
2. 打开 SuperSU，会弹一个获取授权的请求（此刻 su 还归 KingUser 管），点允许；
3. SuperSU 提示"需要更新 SU 二进制" → 点「继续」→ 选 **Normal** 方式，等它把 su 换成自己的；
4. 重启手机；
5. 随便开一个 root 工具验证：授权弹窗应该已经来自 SuperSU，日志里有新记录；
6. 确认无误后，再卸载 KingUser 和 KingRoot。

顺序不能反：**先让 SuperSU 完成接管并验证正常，最后才删 KingUser**。反了会出现 su 没人管的空窗。二进制更新若反复失败，别硬试，回路线 A 走干净重刷。

## 四、不建议碰的第三种做法

社区还流传"脚本一键替换"（跑来历不明的 root.sh）和"手动替换系统文件"（按 CPU 架构手挑二进制、改系统文件权限）两种路子。两者收益与路线 B 相同，但风险高一个量级——公开案例里有人刷成砖。不值得。

## 五、收尾核对

- root 检测工具显示仍是已 root（root 还在，只是换了管家）；
- 授权弹窗来自 SuperSU，日志正常记录；
- KingRoot、KingUser 已从应用列表消失，没有残留图标。

之后的授权、日志、临时 unroot 见 [root权限授权与日常管理.md](root权限授权与日常管理.md)；想彻底退出 root 时见 [完整卸载与取消root方法.md](完整卸载与取消root方法.md)。
