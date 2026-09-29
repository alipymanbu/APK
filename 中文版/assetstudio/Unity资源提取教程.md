# AssetStudio 加载与提取 Unity 资源教程

> 本篇讲怎么把 Unity 资源文件或 AssetBundle 装进 AssetStudio 手机版、怎么提取后再加载，以及每类资源能导成什么格式。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [导出模型与动画教程.md](导出模型与动画教程.md) · [常见问题排查.md](常见问题排查.md) · [游戏资源文件在哪找.md](游戏资源文件在哪找.md)

---

> [!IMPORTANT]
> **AssetStudio 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/336e1a44fb5d](https://pan.quark.cn/s/336e1a44fb5d)

---

## 一、先弄清楚要喂给它什么文件

AssetStudio 解析的是 Unity 引擎产物，常见的有三类：

| 文件 | 常见位置 | 说明 |
| --- | --- | --- |
| 安装包 APK | 下载目录、应用商店缓存 | 资源常打包在 APK 内部的 `assets`、`bin/Data` 目录里 |
| 扩展包 OBB | `Android/obb/包名/` 目录 | 大型游戏的主资源包，一般叫 `main.版本号.包名.obb` |
| AssetBundle / .assets 文件 | 游戏数据目录、热更新缓存目录 | 单独下发的资源包，名字五花八门（带 hash、带版本号） |

把你要看的文件先复制到一个你容易访问的目录（比如 Download），后面加载时少绕路。手机没有 root 时，部分游戏的数据目录访问不到，这种情况只能处理你拿得到的那些文件——具体每个目录怎么进，见 [游戏资源文件在哪找.md](游戏资源文件在哪找.md)。

两个补充：从第三方渠道拿到的 **XAPK** 是个压缩壳，先当 zip 解开、取出里面的 APK 和 OBB 再按上表处理；APK 内部的资源不用装它，用支持解压的文件管理器把 APK 当 zip 解开，取出 `assets`、`bin/Data` 目录就能加载。

## 二、加载：Load file 和 Load folder

打开应用后走菜单 **File - Load file**（选单个文件）或 **File - Load folder**（选整个文件夹）：

- 单个 AssetBundle、单个 OBB → 用 Load file；
- 已经解包出来的一堆资源文件（比如从 APK 里解出的 `bin/Data` 整个目录）→ 用 Load folder，一次性全进来；
- 加载完，主界面会出现两个面板：**Scene Hierarchy**（按场景结构组织）和 **Asset List**（按资源类型平铺），用顶部筛选栏按 Texture2D、AudioClip 等类型过滤更快。

## 三、内存不够？先提取再加载（重要）

这是这款工具最常被问到的坑：**直接加载 AssetBundle 时，文件是在内存里解压读取的**，包一大内存就顶上去，加载慢、手机发热，重则直接被系统杀掉。

正确姿势是**先提取、再从提取结果加载**：

1. 走菜单 **File - Extract 文件**（或 **Extract 文件夹**）；
2. 选一个手机本地目录作为落点，把 AssetBundle 解压出去；
3. 再用 **Load file / Load folder** 改成加载**提取后的文件夹**。

提取后的文件就是普通文件，读取不再整包占内存，大包也能稳住。这个习惯从第一个包开始就养成，能省掉大多数「加载就崩」的问题。

## 四、每类资源能导成什么格式

选中资源（可多选）后走 **Export** 菜单导出。常见类型的对应关系如下：

| 资源类型 | 可导出格式 |
| --- | --- |
| Texture2D（贴图） | PNG、TGA、JPEG、BMP |
| Sprite（精灵图） | 从图集裁切后导出 PNG、TGA、JPEG、BMP |
| AudioClip（音频） | MP3、OGG、WAV；FSB 封装可转成 WAV |
| Font（字体） | TTF、OTF |
| Mesh（网格） | OBJ |
| TextAsset（文本资源） | 原样导出 |
| Shader（着色器） | 文本形式导出 |
| MonoBehaviour | JSON |
| Animator（动画器） | FBX（可带上绑定的动画） |
| MovieTexture / VideoClip | 视频文件 |

具体某一版支持哪些导出格式，以应用内导出菜单实际显示为准。导出前先在预览里确认内容对不对，再决定导单个还是整批。

## 五、批量导出的小技巧

- 在 Asset List 里按类型筛选后，配合 Ctrl / 全选一次导一整类（比如只导所有贴图）；
- 导出目标目录在导出时选择，建议按「游戏名 + 资源类型」建文件夹，回头找图找音频不乱；
- 图集类资源优先看 Sprite 条目而不是 Texture2D——Sprite 是裁好的成品小图，Texture2D 是整张大图。

模型和动画的导出操作步骤更细，单独写在 [导出模型与动画教程.md](导出模型与动画教程.md)；加载报错、导出失败之类的状况去 [常见问题排查.md](常见问题排查.md) 对号入座。

最后提醒一句：提取出来的素材版权归原作品权利方，自己学习研究就好，别拿去二次分发。
