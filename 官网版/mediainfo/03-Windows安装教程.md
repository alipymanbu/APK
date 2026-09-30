# 03 · Windows 安装教程

> **上级索引**：[MediaInfo 下载与安装教程](下载与安装教程.md)
> **相关文档**：[02-下载渠道与版本选择](02-下载渠道与版本选择.md) · [06-命令行使用指南](06-命令行使用指南.md) · [07-图形界面使用指南](07-图形界面使用指南.md) · [08-常见问题与故障排查](08-常见问题与故障排查.md)

---

> [!IMPORTANT]
> **MediaInfo 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7475c96060cb](https://pan.quark.cn/s/7475c96060cb)
>
> 网盘内为**安卓版**：mediainfo 26.05 ｜ 13.64 MB ｜ 包名 `net.mediaarea.mediainfo` ｜ 需安卓 5.2 及以上 ｜ MD5 `69E9E29D3F821916609981F62BBD9F60`

---

## 一、系统要求

| Windows 版本 | 可用 MediaInfo 版本 | 说明 |
| --- | --- | --- |
| **Vista / 7 / 8 / 10 / 11** | 当前最新版（如 26.05） | 官方主支持范围 |
| **XP** | 最高 21.03 | 新版本安装包无法在 XP 上运行 |
| **95 / 98 / 2000** | 最高 0.7.60 | 仅历史版本可用 |

处理器架构：安装包分 **Intel 32 位 / Intel 64 位 / ARM 64 位**。官网提供「通用安装包」，会自动识别架构，**不确定就下通用包**。

---

## 二、先决定装什么

| 需求 | 下载包 | 安装方式 |
| --- | --- | --- |
| 看图省事，GUI + 命令行都要 | `MediaInfo_GUI_<版本>_Windows.exe`（通用安装包） | 安装版 |
| 便携使用 / 无管理员权限 | `MediaInfo_GUI_<版本>_Windows_x64_WithoutInstaller.7z` | 免安装 |
| 只写脚本，不要界面 | `MediaInfo_CLI_<版本>_Windows_x64.zip` | 解压即用 |
| 二次开发 | `MediaInfo_DLL_<版本>_Windows_x64.exe` | 安装版 |

包型的完整对照见 [02-下载渠道与版本选择](02-下载渠道与版本选择.md#三包型对照我该下哪个)。

> `.7z` 是 7-Zip 格式，Windows 自带解压不支持，需装 [7-Zip](https://www.7-zip.org/) 或用支持 7z 的解压工具。`.zip` 可直接右键解压。
>
> 懒得逐个去官网下载？**MediaInfo 安装文件资源**已整理在夸克网盘：[https://pan.quark.cn/s/7475c96060cb](https://pan.quark.cn/s/7475c96060cb)

---

## 三、安装版安装步骤

1. 从官网 Windows 下载页获取 `.exe` 安装程序；
2. **右键 → 以管理员身份运行**（普通双击通常也可以，若提示权限不足则用管理员）；
3. 按向导提示点击下一步；
4. 遇到「选择安装类型 / 组件」时：
   - 需要命令行 → 勾选 **加入 PATH / 添加到系统环境变量** 之类选项；
   - 需要右键菜单集成 → 勾选 **集成到资源管理器（右键菜单 / 拖放）**；
5. 完成安装。

安装完成后，开始菜单会出现 MediaInfo 快捷方式。

---

## 四、便携版（免安装）使用

1. 下载 `..._WithoutInstaller.7z`；
2. 用 7-Zip 解压到任意目录（如 `D:\Tools\MediaInfo\`）；
3. 直接运行目录中的可执行文件即可。

特点：

- **不写注册表、不改系统设置**，删除目录即卸载；
- 可放在 U 盘随身携带；
- 若要在命令行使用，需自行把该目录加入 `PATH`（见第六节）。

---

## 五、只装命令行版（CLI）

1. 下载 `MediaInfo_CLI_<版本>_Windows_x64.zip`；
2. 解压到固定目录，例如 `C:\Tools\MediaInfoCLI\`；
3. 该目录内即为 `MediaInfo.exe`；
4. 按第六节把它加入 `PATH`，之后在任意位置都能直接调用。

> 若同时装了 GUI 版，GUI 安装目录里通常也带一份 CLI，无需重复下载。

---

## 六、把 mediainfo 加入 PATH

### 方法一：安装时勾选（最省事）

重新运行 GUI 安装程序，在组件选择页勾选与 PATH / 环境变量相关的选项。

### 方法二：图形界面手动添加（推荐，最安全）

1. `Win + R` 输入 `sysdm.cpl` 回车；
2. 切到「高级」标签 → 「环境变量」；
3. 在「用户变量」或「系统变量」中找到 `Path`，双击；
4. 点「新建」，填入 MediaInfo 所在目录的**完整路径**（到文件夹，不含文件名）；
5. 一路确定保存。

### 方法三：命令行添加（注意风险）

```powershell
# 仅追加到「用户变量」，不会影响系统其他用户
[Environment]::SetEnvironmentVariable(
  "Path",
  [Environment]::GetEnvironmentVariable("Path", "User") + ";C:\Tools\MediaInfoCLI",
  "User"
)
```

> ⚠️ **不要用 `setx PATH ...` 直接覆盖整个 PATH**：`setx` 有长度截断问题，可能截断并破坏现有 PATH。若一定要用命令，请使用上面的 `[Environment]::SetEnvironmentVariable` 追加方式。

**改完 PATH 后必须重开终端窗口**（包括 Trae / VS Code 内置终端）才会生效。

---

## 七、验证安装

打开**新的** PowerShell 或 CMD 窗口：

```powershell
mediainfo --Version
```

期望输出类似：

```
MediaInfo Command line,
MediaInfoLib - v26.05
```

再试一个文件：

```powershell
mediainfo "D:\Videos\sample.mkv"
```

能打印出 General / Video / Audio 分组信息，即安装成功。

**图形界面验证**：开始菜单打开 MediaInfo，主窗口出现即为成功。

---

## 八、卸载

| 安装方式 | 卸载方法 |
| --- | --- |
| 安装版 | 「设置 → 应用」中找到 MediaInfo 卸载；或运行安装目录下的卸载程序 |
| 便携版 / CLI 版 | 直接删除所在文件夹；若曾手动加过 `PATH`，记得把该项一并删除 |

---

## 九、常见坑

**1. 装完只有界面，命令行提示找不到 `mediainfo`**
安装时没勾选 PATH 选项，或勾选了但没重开终端。解决见第六节 + [08-常见问题与故障排查](08-常见问题与故障排查.md)。

**2. 解压 `.7z` 失败**
Windows 自带解压不支持 7z，请安装 7-Zip。

**3. 提示「Windows 已保护你的电脑」**
新版本安装包首次运行时的 SmartScreen 提示。确认文件来自官网后，点「更多信息 → 仍要运行」。

**4. 32 位系统装了 64 位包**
会直接报错无法运行。请回到官网下载 32 位（i386）包。

**5. 命令行输出中文乱码**
多见于旧版 CMD 的代码页问题，可先执行 `chcp 65001` 切到 UTF-8，或改用 Windows Terminal / PowerShell。

---

## 十、下一步

- 重新获取安装包 → [MediaInfo 安装文件资源（夸克网盘）](https://pan.quark.cn/s/7475c96060cb)
- 用命令行批处理 → [06-命令行使用指南](06-命令行使用指南.md)
- 用界面看文件 → [07-图形界面使用指南](07-图形界面使用指南.md)
