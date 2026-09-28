# Suunto GPX 与 FIT 文件导出导入

> 本篇讲把锻炼记录从 Suunto App 导出成 GPX / FIT 文件、把路线 GPX 分享或导入回 app 的操作，以及两种格式的差别。
> **相关文档**：[路线规划与户外地图.md](路线规划与户外地图.md) · [运动数据分析与第三方服务.md](运动数据分析与第三方服务.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **Suunto 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/48faeea96f10](https://pan.quark.cn/s/48faeea96f10)

---

## 一、三种导出，先选对格式

官方支持页（[us.suunto.com/pages/what-type-of-files-can-i-export-from-the-suunto-app](https://us.suunto.com/pages/what-type-of-files-can-i-export-from-the-suunto-app)）列出的格式与内容差别：

| 导出 | 内容 | 适合 |
| --- | --- | --- |
| 锻炼 → GPX | 位置 + 时间（有心率数据时也带上） | 导进其他平台看轨迹；不含 app 里的全部数据 |
| 路线 → GPX | 只有位置（你锻炼的轨迹线） | 给别的服务当导航线用；可从锻炼记录或路线库导出 |
| 锻炼 → FIT | 位置、时间、时间戳、心率、功率、步频等全部 | 官方推荐的导出格式，各平台兼容最好 |

注意：**没有 GPS 的记录导不了 GPX**，只能导 FIT（官方 FAQ 明确标注）。

## 二、导出步骤

1. 打开 Suunto App 里任意一条锻炼记录；
2. 点右上角三个点；
3. 选要导出的格式；
4. 存到手机，或直接用兼容应用打开。

路线库里的路线另有一条分享路径：打开已保存路线 → 点分享按钮 → 发给朋友、AirDrop、存到「文件」或 Google Drive 都可以。

## 三、导入 GPX

手里已经有 .gpx 轨迹（别人分享的、其他平台导出的）时，可以导入 app 变成自己的路线，官方分平台给了图文步骤：

- iOS：[us.suunto.com/pages/how-do-i-import-a-gpx-file-in-suunto-app-for-ios](https://us.suunto.com/pages/how-do-i-import-a-gpx-file-in-suunto-app-for-ios)
- Android：[us.suunto.com/pages/how-do-i-import-a-gpx-file-in-suunto-app-for-android](https://us.suunto.com/pages/how-do-i-import-a-gpx-file-in-suunto-app-for-android)

导入后的路线和自建路线一样能同步到腕表导航；路线的搜索、筛选、排序与管理见 [路线规划与户外地图.md](路线规划与户外地图.md)。

## 四、什么时候用哪条路

- 平时想把锻炼同步到 Strava 这类平台：绑服务自动同步最省事，见 [运动数据分析与第三方服务.md](运动数据分析与第三方服务.md)；
- 偶尔要把某一条记录给到没绑定的平台，或想本地留底：手动导出 FIT；
- 只想把轨迹线给朋友照着跑：从路线库分享 GPX。

导出入口找不到、文件打不开这类问题，先看 [常见问题与排查.md](常见问题与排查.md)。
