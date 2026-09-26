# 和BiliPai是同一个软件吗

> 讲 BiliPure 与名字极其相似的 BiliPai 是不是同一个东西、为什么会混、怎么核对装对了。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [和官方哔哩哔哩App有什么区别.md](和官方哔哩哔哩App有什么区别.md)

---

> [!IMPORTANT]
> **BiliPure 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/d8ef8be45399](https://pan.quark.cn/s/d8ef8be45399)

---

## 一、结论：不是同一个软件

BiliPure 和 BiliPai 是**两款不同的第三方哔哩哔哩客户端**，开发者、安装包、包名都不一样。它们常被各下载站放在同一页互相推荐，名字又只差三个字母，装错了很常见 —— 装错不是坏事，但你要按另一款的说明去排错就会对不上。

## 二、一眼对照

| | BiliPure | BiliPai |
| --- | --- | --- |
| 包名 | `com.bilibili.pure` | `com.android.purebilibili` |
| 本文对应的版本 / 大小 | 1.0 / 19.80M | 各分发版本不一，以下载页当时显示为准 |
| 界面重心 | 打开即搜索，无推荐流 | 有首页推荐流、动态等板块 |
| 登录方式 | 扫码 + 账号密码 | 扫码 + 网页授权登录 |

两者都会宣称「无广告、轻量」，所以从宣传语上根本分不出来，**只能靠包名分辨**。

## 三、30 秒核对法

1. 手机**设置 → 应用 → 应用管理**，找到装好的应用点进去。
2. 看「应用信息」里的包名：
   - `com.bilibili.pure` → 装的是 BiliPure，本目录的其他文档都适用；
   - `com.android.purebilibili` → 装的是 BiliPai，说明与设置去找对应资料。
3. 顺手对一下版本（1.0）和大小（约 19.8 MB）是否与预期一致。

嫌翻设置麻烦，也可以在电脑上看 APK 文件名：BiliPure 这份叫 `BiliPure_v1.0.apk`。

## 四、核对安装文件本身（防换包）

拿到的 APK 跟宣称的对不上，就别装。把文件传到 Windows 电脑上，在文件所在目录打开 PowerShell 执行：

```powershell
certutil -hashfile ".\BiliPure_v1.0.apk" MD5
```

输出应为 `91fe870d47e2abef00e07084c07d76d3`（不区分大小写）。与网盘里这份的规格一致，才说明文件完整没被替换：

- 版本 1.0、大小 19.80M、包名 `com.bilibili.pure`。

## 五、从这里拿安装文件

认准 [BiliPure 安装文件资源（夸克网盘）](https://pan.quark.cn/s/d8ef8be45399)，转存后下载 `BiliPure_v1.0.apk` 即可；下载与安装步骤见 [下载与安装教程.md](下载与安装教程.md)。
