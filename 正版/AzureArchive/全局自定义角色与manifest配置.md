# 全局自定义角色与manifest配置

> 在项目里导入的自定义角色只对一个项目生效；要所有项目都能用，得动数据目录里的 overrides 文件夹和 manifest.json。本篇讲这两条路怎么走。
> **相关文档**：[自定义角色导入方法.md](自定义角色导入方法.md) · [工程文件备份与转移.md](工程文件备份与转移.md)

---

> [!IMPORTANT]
> **AzureArchive 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ff41b2ade19d](https://pan.quark.cn/s/ff41b2ade19d)

---

## 一、先分清全局与本地两套文件夹

自定义资源涉及两套结构完全一致的文件夹：

| 文件夹 | 生效范围 | 位置 |
| --- | --- | --- |
| 全局自定义文件夹 | 所有项目和存档 | 数据目录下的 `overrides/` |
| 本地自定义文件夹 | 只对同名项目（或存档）生效 | `projects/` 里与项目文件同名的文件夹 |

本地文件夹更易于移植——拷项目时带上它，自定义资源就跟着走。数据目录各平台的具体路径见[工程文件备份与转移.md](工程文件备份与转移.md)。

## 二、自动自定义按钮（0.5.0 及以上）

各选择器（角色、音乐、弹出图片等）左上角有个背包图标，点它即可把对应资源快速导入。两点说明：

- 它只是快速导入入口，角色文件仍然要你自己做好，它不会帮你制作角色。
- 通过它导入的资源属于本地自定义资源；安卓、iOS 上可能因权限问题无法正确操作资源文件，失败就转下面的手动方式。

## 三、manifest.json：手动配置的入口

在自定义资源文件夹里新建 `manifest.json`，先把四个空字段写上：

```json
{
  "CharacterOverrides": [],
  "PopupOverrides": [],
  "SoundOverrides": [],
  "BgOverrides": []
}
```

四个字段各管一类资源：

| 字段 | 对应资源 | 格式要求 |
| --- | --- | --- |
| `PopupOverrides` | 弹出图片 | PNG，填相对 overrides 文件夹的路径（带扩展名） |
| `SoundOverrides` | 音效 | OGG、MP3、WAV |
| `BgOverrides` | 背景图片 | PNG |
| `CharacterOverrides` | 自定义角色 | 见下一节 |

比如 `overrides` 文件夹里有 `koharu.png`、`overrides/yuzu` 文件夹里有 `0721.png`，想把二者作为弹出图片，就把 `PopupOverrides` 写成：

```json
"PopupOverrides": [
  "koharu.png",
  "yuzu/0721.png"
]
```

## 四、加一个有立绘的新角色

往 `CharacterOverrides` 里加一个对象，字段如下：

| 字段 | 填什么 |
| --- | --- |
| `Identifier` | 角色标识，不要与现有角色重复 |
| `Name` | 角色名字，显示在剧情里 |
| `Nickname` | 昵称，名字旁边的小字 |
| `SpinePortraitPath` | spine 立绘文件相对 overrides 的路径，不含后缀 |
| `SmallPortraitPath` | PNG 格式头像相对 overrides 的路径 |

立绘文件是三件套（`xx.skel`、`xx.atlas`、`xx.png`），除后缀外文件名必须一致。立绘动画按官方规范命名：待机 `Idle_01`、眨眼 `Eye_Close_01`（可以没有）、表情用两位数字（`00`、`01`……`99`），眨眼动画只在无表情和 `01` 表情时播放。头像需要特定的分辨率与裁切才能正常显示，官方文档提供了样例包下载，照着样例做最稳。

所有字段不要留空。spine 版本坑（3.8.75 不可用、推荐 3.8.96）与项目内导入是一致的，见[自定义角色导入方法.md](自定义角色导入方法.md)。

## 五、无立绘角色与给现有角色改名

- **无立绘新角色**：`CharacterOverrides` 里只填 `Identifier`、`Name`、`Nickname`，再加一个 `CharacterReference` 字段，值照抄 `???` 即可。
- **给现有角色改名字、昵称**：同样走 `CharacterOverrides`，把 `CharacterReference` 填成目标角色的 ID。只想做别名、不动立绘时用这个写法。

## 六、动手之前

先把 overrides 文件夹整份备份——要恢复原状时把备份放回去，或按手动自定义方式把原有资源导回来。本篇字段与格式均为官方文档当前口径，细节以后以官方页面为准。
