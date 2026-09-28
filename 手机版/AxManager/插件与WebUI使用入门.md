# AxManager 插件与 WebUI 使用入门

> AxManager 的「非 Root 模块」体系：插件怎么装、module.prop 怎么认、哪些 Root 模块的玩法在这里不生效，以及在浏览器里执行命令的 WebUI。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [常见问题与启动失败排查.md](常见问题与启动失败排查.md) · [无线调试启动与配对教程.md](无线调试启动与配对教程.md)

---

> [!IMPORTANT]
> **AxManager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e76a7ed9b163](https://pan.quark.cn/s/e76a7ed9b163)

---

## 一、先说清楚边界

AxManager 的插件（官方叫 Unrooted Module）沿用了 Root 模块（Magisk / KernelSU）的包结构，但它运行在 ADB 权限层，不是真 Root：

- 需要对 `/system` 做深度改动的 Root 模块，在这里装了也达不到原效果——目前插件环境只把插件自带的 `/system/bin` 加进 PATH。
- 给 AxManager 装插件不等于给手机刷了模块，卸载也只影响 AxManager 这一层。
- 官方对插件的定位是「探索阶段的功能」，装第三方插件前留意插件作者的更新说明。

## 二、装一个插件

插件目前**没有官方集中分发渠道**：官方文档只定义了格式，没有插件市场。常见来源是 GitHub（搜 `AxManager plugin` / `Axeron plugin`）和玩机社区的分享帖。

拿到 zip 后先过三道判据再装：

1. 包里有 `module.prop`（没有就不是插件）。
2. `module.prop` 里的 `axeronPlugin` 数值不超过你手机上的服务版本（见第三节）。
3. 来源可信：优先选能对上源码仓库、有更新记录的作品，社区随手转发的 zip 谨慎对待。

安装操作：

1. 在 AxManager 里打开插件安装器，选择这个 zip 安装。
2. 装完在插件列表里启用或停用。

## 三、认得 module.prop（挑插件时用得上）

插件包里必须有 `module.prop`，否则不会被识别为插件。字段一览：

| 字段 | 说明 |
| --- | --- |
| id | 插件唯一标识：字母开头，只能含字母、数字、点、下划线、连字符；不能带空格、不能数字开头 |
| name / version / author / description | 单行文本 |
| versionCode | 整数，用来比较版本 |
| axeronPlugin | 目标 AxManager 服务版本号（整数） |

关键判据：**axeronPlugin 的数值必须 ≤ 你手机上 AxManager 服务的版本号，超过就装不上**。遇到装不上的插件，先看这个字段；id 的命名规则（空格、数字开头）也是常见的翻车点。

## 四、插件的启停与手动管理

- 停用某个插件：在它的目录里放一个名为 `disable` 的空文件；放 `remove` 则是下次重启后移除。
- 插件目录在 `/data/user_de/0/com.android.shell/axeron/plugins/`，每个插件一个以 id 命名的子目录；AxManager 内置的 BusyBox 在 `/data/user_de/0/com.android.shell/axeron/bin/busybox`。
- 插件可带的脚本：`post-fs-data.sh`、`service.sh`（开机后执行），`action.sh`（你在应用里点「执行」按钮时跑），`uninstall.sh`（移除时跑）。

## 五、WebUI：浏览器里操作

带 WebUI 的插件提供网页界面，在 AxManager 里打开对应插件即可进入，用浏览器完成交互和命令执行，不需要再开电脑。WebUI 的写法与 KernelSU 的模块 WebUI 规范一致，插件作者可以参考 KernelSU 的 [模块 WebUI 文档](https://kernelsu.org/guide/module-webui.html)。

## 六、给想自己写脚本或模块的人

- AxManager 内置的 BusyBox 直接用 Magisk 项目编译的同一个二进制（带完整 SELinux 支持），所以 Magisk / KernelSU 模块里的 BusyBox 脚本可以原样跑。
- 所有插件脚本都运行在 BusyBox 的 ash 里，并启用了 Standalone Shell 模式：`ls`、`rm`、`chmod` 这类命令一律优先用 BusyBox 内置版本，不受 PATH 影响。想让某条命令走系统原版，用完整路径调用它。
- 脚本里可以用环境变量 `AXERON`（值为 true）判断当前跑在 AxManager 还是 Magisk / KernelSU 里，做分支处理。
- 取插件自身路径统一写 `MODDIR=${0%/*}`，不要写死路径（`customize.sh` 除外）。

## 七、装不上或不生效

- 装不上：先核对第三节说的 `axeronPlugin` 与 id 命名规则；服务版本太旧的话，升级 AxManager 再装。
- 装上不生效：检查是不是触到了第一节的边界（`/system` 深度改动在这里做不了）。
- 插件列表全空或全部失效：多半是服务没在跑，先按 [常见问题与启动失败排查.md](常见问题与启动失败排查.md) 把服务启动起来，再回来看插件。
