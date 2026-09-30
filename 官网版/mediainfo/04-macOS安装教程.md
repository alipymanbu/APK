# 04 · macOS 安装教程

> **上级索引**：[MediaInfo 下载与安装教程](下载与安装教程.md)
> **相关文档**：[02-下载渠道与版本选择](02-下载渠道与版本选择.md) · [06-命令行使用指南](06-命令行使用指南.md) · [07-图形界面使用指南](07-图形界面使用指南.md) · [08-常见问题与故障排查](08-常见问题与故障排查.md)

---

> [!IMPORTANT]
> **MediaInfo 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7475c96060cb](https://pan.quark.cn/s/7475c96060cb)
>
> 网盘内为**安卓版**：mediainfo 26.05 ｜ 13.64 MB ｜ 包名 `net.mediaarea.mediainfo` ｜ 需安卓 5.2 及以上 ｜ MD5 `69E9E29D3F821916609981F62BBD9F60`

---

## 一、选择安装方式

macOS 上有两条路，**可以同时用**（界面版 + 命令行版共存）：

| 方式 | 命令 / 文件 | 得到什么 | 适合谁 |
| --- | --- | --- | --- |
| **官方 dmg** | `MediaInfo_<版本>_Mac_OS.dmg` | 图形界面应用 | 偶尔查看文件的普通用户 |
| **Homebrew cask** | `brew install --cask mediainfo` | 图形界面应用 | 已装 Homebrew，想统一管理 |
| **Homebrew formula** | `brew install media-info` | **命令行工具** | 写脚本、批处理 |

> AI 提示：Homebrew 里 GUI 用的是 cask 名 `mediainfo`，CLI 用的是 formula 名 `media-info`（带连字符），两者名字不同，别搞混。
>
> 不想走包管理器？也可从夸克网盘获取 **MediaInfo 安装文件资源**：[https://pan.quark.cn/s/7475c96060cb](https://pan.quark.cn/s/7475c96060cb)

---

## 二、方式一：官方 dmg 安装

1. 从官网 macOS 下载页获取 `.dmg` 文件；
2. 双击打开 dmg，会看到 MediaInfo 图标和「应用程序」文件夹的快捷方式；
3. 把 **MediaInfo 图标拖入「应用程序」文件夹**；
4. 在启动台或「应用程序」中打开。

### 首次打开被拦截怎么办

macOS 的 Gatekeeper 会对未经过公证（notarization）的应用弹窗拦截，提示「无法打开，因为无法验证开发者」。处理方式：

1. 打开 **系统设置 → 隐私与安全性**；
2. 在页面下方找到关于 MediaInfo 被阻止的提示；
3. 点击 **「仍要打开」**，再确认一次。

或者：在「应用程序」里**右键点击** MediaInfo → 选择「打开」→ 在弹窗中确认「打开」。此操作每个应用只需做一次。

---

## 三、方式二：Homebrew 安装

### 安装图形界面版

```bash
brew install --cask mediainfo
```

### 安装命令行版

```bash
brew install media-info
```

### 一次装齐（推荐给开发者）

```bash
brew install --cask mediainfo && brew install media-info
```

### 升级与卸载

```bash
# 升级
brew upgrade --cask mediainfo
brew upgrade media-info

# 卸载
brew uninstall --cask mediainfo
brew uninstall media-info
```

> Homebrew 的 formula 版本可能略滞后于官网当月最新版，这是正常现象；追求最新版请走官方 dmg 或官网的独立安装包。

---

## 四、验证安装

打开终端（Terminal / iTerm2）：

```bash
mediainfo --Version
```

期望输出类似：

```
MediaInfo Command line,
MediaInfoLib - v26.05
```

再试一个文件：

```bash
mediainfo ~/Movies/sample.mkv
```

能打印 General / Video / Audio 分组即成功。

> 若提示 `command not found: mediainfo`，说明你装的是 GUI（cask）而不是 CLI（formula），执行 `brew install media-info` 补上；或参考 [08-常见问题与故障排查](08-常见问题与故障排查.md)。

**图形界面验证**：启动台打开 MediaInfo，主窗口出现即为成功。

---

## 五、Apple Silicon 与 Intel 说明

官网 macOS 安装包与 Homebrew 均同时覆盖 Apple Silicon（M 系列）与 Intel 机型：

- 走 **Homebrew** 时，Homebrew 会自动匹配当前架构，无需手动选择；
- 走 **官方 dmg** 时，若无特殊说明，下载页提供的即通用版本。

---

## 六、卸载

| 安装方式 | 卸载方法 |
| --- | --- |
| 官方 dmg | 打开「应用程序」文件夹，把 MediaInfo 拖到废纸篓；再到「访达 → 前往 → 前往文件夹」输入 `~/Library/Preferences`，删除残留的 MediaInfo 偏好设置文件（可选） |
| Homebrew | 执行上文对应的 `brew uninstall` 命令 |

---

## 七、常见坑

**1. `brew install mediainfo` 报找不到 formula**
GUI 是 cask（需 `--cask`），CLI 的 formula 名是 `media-info`。命令写错是最常见原因。

**2. 拖入「应用程序」后双击无反应**
Gatekeeper 拦截，按第二节的放行步骤处理。

**3. 终端里 `mediainfo` 和多版本冲突**
若同时用 Homebrew 与官方 dmg，可能装了两份。用 `which -a mediainfo` 查看全部路径，按需清理。

**4. 输出中文或字段名被翻译**
macOS 会按系统语言显示。脚本解析时建议加 `--Language=raw` 固定字段名，见 [06-命令行使用指南](06-命令行使用指南.md)。

---

## 八、下一步

- 重新获取安装包 → [MediaInfo 安装文件资源（夸克网盘）](https://pan.quark.cn/s/7475c96060cb)
- [06-命令行使用指南](06-命令行使用指南.md)
- [07-图形界面使用指南](07-图形界面使用指南.md)
