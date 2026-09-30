# 05 · Linux 安装教程

> **上级索引**：[MediaInfo 下载与安装教程](下载与安装教程.md)
> **相关文档**：[02-下载渠道与版本选择](02-下载渠道与版本选择.md) · [06-命令行使用指南](06-命令行使用指南.md) · [08-常见问题与故障排查](08-常见问题与故障排查.md)

---

> [!IMPORTANT]
> **MediaInfo 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/7475c96060cb](https://pan.quark.cn/s/7475c96060cb)
>
> 网盘内为**安卓版**：mediainfo 26.05 ｜ 13.64 MB ｜ 包名 `net.mediaarea.mediainfo` ｜ 需安卓 5.2 及以上 ｜ MD5 `69E9E29D3F821916609981F62BBD9F60`

---

## 一、先搞清两个包名

Linux 上 MediaInfo 通常拆成两个包，**别只装错那一个**：

| 包名 | 内容 |
| --- | --- |
| `mediainfo` | **命令行工具**（脚本、CI 环境只需要这个） |
| `mediainfo-gui` | 图形界面 |

想要界面就两个都装；只做自动化就装 `mediainfo` 一个即可。

发行版仓库没有收录、或安装遇到依赖冲突时，也可直接取用 [MediaInfo 安装文件资源（夸克网盘）](https://pan.quark.cn/s/7475c96060cb)。

---

## 二、按发行版安装

### Debian / Ubuntu 系

```bash
sudo apt update
sudo apt install mediainfo mediainfo-gui
```

只装命令行：

```bash
sudo apt install mediainfo
```

### Fedora

```bash
sudo dnf install mediainfo mediainfo-gui
```

### RHEL / CentOS / AlmaLinux / Rocky Linux

```bash
# 先启用 EPEL 源
sudo dnf install epel-release
sudo dnf install mediainfo mediainfo-gui
```

### Arch Linux / Manjaro

```bash
sudo pacman -S mediainfo mediainfo-gui
```

> Manjaro 上的包由社区维护，官网标注为「不支持」级别，遇到问题请走 AUR 或 AppImage。

### openSUSE

```bash
sudo zypper install mediainfo mediainfo-gui
```

> openSUSE 还有 Packman 源可提供更新版本。

### 其他发行版

Mageia、SLE、Solaris、Raspbian、Lambda 等平台官方均有对应 releases 页，见[下载总页](https://mediaarea.net/en/MediaInfo/Download)。Gentoo / Slackware / FreeBSD 属于社区或非官方维护。

---

## 三、要更新版本：使用 MediaArea 官方仓库

发行版仓库里的版本**通常滞后数月甚至数年**。需要最新版时，可以添加 MediaArea 官方仓库：

- 官方仓库配置说明：<https://mediaarea.net/en/Repos>

添加后按发行版正常升级即可（如 `sudo apt update && sudo apt upgrade mediainfo`）。

> 生产环境建议先在测试机验证新版输出差异，再批量升级（原因见 [08-常见问题与故障排查](08-常见问题与故障排查.md)）。

---

## 四、免依赖安装：多种通用包

不想动包管理器时，可选用官方提供的通用分发格式：

| 形式 | 特点 | 适用场景 |
| --- | --- | --- |
| **AppImage** | 单文件、双击即运行、不侵入系统 | 临时使用、无 root 权限 |
| **Flatpak** | 沙箱化，依赖自带 | 桌面环境统一管理 |
| **Snap** | 自动更新 | Ubuntu 系桌面 |
| **`.deb` / `.rpm`** | 官方独立安装包 | 需要官方版本但不想加仓库 |
| **`.tar.xz` 二进制** | 解压即用 | 服务器 / 容器 / 精简系统 |

### deb / rpm 直接安装

```bash
# Debian / Ubuntu
sudo dpkg -i mediainfo_<版本>_amd64.deb

# RHEL / Fedora
sudo rpm -ivh mediainfo-<版本>.x86_64.rpm
```

### tar.xz 通用二进制

```bash
tar -xf mediainfo_<版本>_Linux_x64.tar.xz
cd mediainfo_<版本>_Linux_x64
./mediainfo --Version
```

按需把目录加入 `PATH`，或直接建立软链接：

```bash
sudo ln -s "$PWD/mediainfo" /usr/local/bin/mediainfo
```

### AppImage

```bash
chmod +x MediaInfo_<版本>.AppImage
./MediaInfo_<版本>.AppImage
```

---

## 五、验证安装

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
mediainfo ~/Videos/sample.mkv
```

能打印 General / Video / Audio 分组即成功。

图形界面验证：应用菜单中能找到 MediaInfo，或在终端执行 `mediainfo-gui`。

---

## 六、卸载

```bash
# Debian / Ubuntu
sudo apt remove mediainfo mediainfo-gui

# Fedora / RHEL 系
sudo dnf remove mediainfo mediainfo-gui

# Arch
sudo pacman -Rns mediainfo mediainfo-gui

# openSUSE
sudo zypper remove mediainfo mediainfo-gui
```

通用包（AppImage / tar.xz）直接删除文件即可；用 `ln -s` 建过软链接的记得一并删除。

---

## 七、服务器 / 容器场景建议

在无图形界面的服务器或 CI 容器里，**只装 CLI**，体积最小：

```dockerfile
# Debian / Ubuntu 基础镜像示例
RUN apt-get update && apt-get install -y --no-install-recommends mediainfo \
    && rm -rf /var/lib/apt/lists/*
```

```dockerfile
# Alpine 类镜像（若有对应包）
RUN apk add --no-cache mediainfo
```

常见用法：在转码流水线中先跑一次 `mediainfo --Output=JSON`，把结果交给后续步骤判断。

---

## 八、常见坑

**1. `mediainfo: command not found`，但图形界面能用**
只装了 `mediainfo-gui`。补装 `mediainfo`。

**2. 装了但版本很旧**
发行版仓库滞后所致，改用[官方仓库](#三要更新版本使用-mediaarea-官方仓库)或 AppImage。

**3. 中文文件名 / 输出乱码**
确认系统 `LANG` / `LC_ALL` 为 UTF-8 环境（如 `zh_CN.UTF-8` 或 `C.UTF-8`）。脚本解析建议加 `--Language=raw`。

**4. 依赖冲突**
常见于直接 `dpkg -i` 官方 deb。用 `sudo apt -f install` 修复依赖，或改用 tar.xz / AppImage。

**5. 无 root 权限**
用 AppImage 或 tar.xz 版，解压到用户目录，把目录加进自己的 `~/.bashrc` 的 `PATH`。

---

## 九、下一步

- 直接取安装包 → [MediaInfo 安装文件资源（夸克网盘）](https://pan.quark.cn/s/7475c96060cb)
- [06-命令行使用指南](06-命令行使用指南.md)（Linux 上最常用）
- [07-图形界面使用指南](07-图形界面使用指南.md)
