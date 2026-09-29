# adb命令手动修改教程

> 不装应用也能去感叹号：用电脑 ADB 直接改系统的检测地址，一条命令的事。适合不想 root、不想折腾 Shizuku，或者系统较老（安卓 10 及以下）用不了无线调试启动的设备。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [wifi感叹号是什么原因.md](wifi感叹号是什么原因.md) · [常见问题与解决方法.md](常见问题与解决方法.md)

---

> [!IMPORTANT]
> **CaptiveMgr 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/c8d8c83275d5](https://pan.quark.cn/s/c8d8c83275d5)

---

## 一、为什么还留着手改这一条路

CaptiveMgr 改的就是下面这组系统参数，应用只是替你把这些命令执行了一遍。以下情况你会需要原始命令：设备是安卓 10 及以下（无线调试方式要求安卓 11+，Shizuku 那条路走不通又不想连电脑反复启动）；或者你只是临时修一次，不想为一条命令装两个应用。手改和应用改效果完全一样，改完同样持久生效。

## 二、准备工作

1. 电脑上下载谷歌官方的 SDK Platform Tools（搜索"SDK Platform Tools download"进谷歌开发者页面下载对应系统的压缩包），解压到任意文件夹。
2. 手机上开启开发者选项：设置 → 关于手机 → 连点"版本号"7 次（不同机型入口略有差异）。
3. 开发者选项里打开"USB 调试"，数据线连电脑，手机弹窗选允许（可勾选"始终允许"）。
4. 在解压目录里打开终端（Windows 可在文件夹地址栏输入 `cmd` 回车），执行：

```bash
adb devices
```

能看到设备序列号和 `device` 字样即连接成功。以下命令都在这个窗口里执行。

## 三、先认清你系统的参数格式

这项机制的参数名在历史版本里换过几轮，用错参数名不会报错，但等于什么都没改。对照表（覆盖安卓 5.0 至 9，更新的系统机制延续，以系统实际为准）：

| 安卓版本 | 服务器地址参数 | 开关参数 |
| --- | --- | --- |
| 5.0 – 6.x | `captive_portal_server`（只填主机名） | — |
| 7.0 – 7.1 | `captive_portal_server`（只填主机名） | `captive_portal_use_https`（默认走 HTTPS） |
| 7.1.1 起 | `captive_portal_http_url` + `captive_portal_https_url`（填完整 URL） | 同上 |
| 7.1.2 起（含 8.x / 9.x） | 同上 | `captive_portal_mode` |

## 四、改地址的命令

以换成小米的检测地址为例。

**安卓 7.1.1 及以上**（地址要写完整 URL，HTTP 和 HTTPS 两条都写）：

```bash
adb shell settings put global captive_portal_http_url http://connect.rom.miui.com/generate_204
adb shell settings put global captive_portal_https_url https://connect.rom.miui.com/generate_204
```

**安卓 5.0 – 7.1**（只写主机名，不带 `http://` 和路径）：

```bash
adb shell settings put global captive_portal_server connect.rom.miui.com
```

改完开关一次飞行模式，或重启 WiFi，让系统重新探测。想确认是否写入成功，用 `get` 查：

```bash
adb shell settings get global captive_portal_https_url
```

另外两个开关，多数人用不到但要知道：`adb shell settings put global captive_portal_mode 0`（7.1.2+）可以彻底关掉检测，图标永远干净，代价是要登录的 WiFi 不再自动弹登录页（影响见 [常见问题与解决方法.md](常见问题与解决方法.md) 第七节）；7.0 – 7.1 上的老开关叫 `captive_portal_use_https`，写 `0` 禁用、写 `1` 启用。

## 五、恢复默认

把改过的参数删掉就回到系统出厂状态（删除后变量回归默认值）：

```bash
adb shell settings delete global captive_portal_http_url
adb shell settings delete global captive_portal_https_url
adb shell settings delete global captive_portal_server
```

哪个改过就删哪个，没改过的执行了也没有副作用。

## 六、嫌命令麻烦

上面这套参数，[CaptiveMgr 安装文件资源（夸克网盘）](https://pan.quark.cn/s/c8d8c83275d5) 里的应用会替你执行——选服务器、点应用、刷新图标三步完成，还带服务器测速和一键恢复默认；安装与授权见 [下载与安装教程.md](下载与安装教程.md)。检测地址为什么必须换，背景在 [wifi感叹号是什么原因.md](wifi感叹号是什么原因.md)。
