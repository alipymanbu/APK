# Tricky Store target.txt 配置详解

> `target.txt` 决定 Tricky Store 对哪些应用生效，是刷入后最常改的一个文件。本篇讲它的路径、写法、包名后缀 `?` 与 `!` 的区别，以及怎么查应用的包名。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [keybox.xml配置指南.md](keybox.xml配置指南.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **Tricky Store 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a8f462b3607c](https://pan.quark.cn/s/a8f462b3607c)

---

## 一、文件在哪、长什么样

路径固定为：

```text
/data/adb/tricky_store/target.txt
```

内容就是一行一个应用包名，`#` 开头的行是注释，可以留在文件里当说明。官方文档给出的示例原样贴在这里，你刷完第一次打开这个文件时看到的大致就是它：

```text
# target.txt
# 对 KeyAttestation App 使用自动模式
io.github.vvb2060.keyattestation
# 对 momo 使用修改证书链模式
io.github.vvb2060.mahoshojo?
# 对 gms 使用生成证书链模式
com.google.android.gms!
```

刷入后模块会自带一份默认的 `target.txt`，你可以直接在后面追加，也可以整份替换成自己的。**编辑它需要带 root 权限的文件管理器**（如 MT 管理器），普通文件管理器看不到 `/data/adb/` 这个目录。

## 二、包名后面的 `?` 和 `!`

Tricky Store 修改证书链有两种工作模式，`target.txt` 里的后缀决定某个应用走哪一种：

| 写法 | 模式 | 什么时候用 |
| --- | --- | --- |
| 包名后不加后缀 | 自动选择 | 默认。模块自己判断用哪种模式 |
| 包名后加 `?` | 强制修改证书链模式 | 只改 TEE 返回的叶证书。TEE 正常的设备上一般够用 |
| 包名后加 `!` | 强制生成证书链模式 | 用 `keybox.xml` 里的密钥材料生成整条证书链。TEE 损坏的设备，或自动模式在该应用上不奏效时 |

两点补充：

- 设备的 TEE（可信执行环境）损坏时，模块**自动**切到生成证书链模式，不需要你手动加 `!`；`!` 是给「想对某个应用强制指定」的场景用的。
- 后缀紧跟在包名后面，中间不能有空格。

## 三、保存即生效

官方明确说明所有配置**立即生效**，`target.txt` 改完保存就行，不用重启模块、也不用重启手机。如果你改了却没变化，先检查是不是这些原因：路径写错（有人建到了 `/data/adb/tricky_store` 的子目录里）、行尾留了多余空格、把包名写成了应用显示名——排查细节见 [常见问题与排查.md](常见问题与排查.md)。

## 四、怎么查一个应用的包名

`target.txt` 只认包名（形如 `com.google.android.gms`），不认应用显示名。常用三种查法：

1. **包名查看器类应用**：应用商店里搜「包名查看器」，装一个，点开任何应用都能看到它的包名；
2. **root 文件管理器**：到 `/data/app/` 目录下看应用目录名，里面带着包名；
3. **adb 命令**：手机连电脑开 USB 调试后，`adb shell pm list packages | grep 关键词` 直接过滤。

把查到的包名按「一行一个」追加到 `target.txt` 末尾即可，条数没有官方限制。

嫌手动维护麻烦的话，有社区维护的辅助模块（如 Tricky Addon）可以帮你更新常用包名列表——这是第三方项目，不是 Tricky Store 本体的一部分，用不用、信到什么程度自己判断，以[该项目页面](https://github.com/KOWX712/Tricky-Addon-Update-Target-List)为准。

## 五、改完怎么验证

验证方式是把检测类应用加进 `target.txt` 再跑一遍，最直接的两个：

- **KeyAttestation**（`io.github.vvb2060.keyattestation`）——官方示例里用它演示自动模式，直接查看密钥证明的证书链结果；
- **momo**（`io.github.vvb2060.mahoshojo`）——官方示例里对它强制使用修改证书链模式，用于检查环境。

这两个应用本身就是很好的「效果确认器」：改完 `target.txt` 后打开它们跑一次，对比结果有没有变化，比在别的应用里反复试错快得多。
