# Hypic AI 扩图提示词怎么写（附现成可抄的场景模板）

> 本篇讲 Hypic AI 扩图（AI Expand）里提示词的输入位置、写作公式，以及按场景整理的现成英文提示词，复制即可用。
> **相关文档**：[AI修图功能怎么用](AI修图功能怎么用.md) · [常见问题排查](常见问题排查.md) · [是什么软件](是什么软件.md)

---

> [!IMPORTANT]
> **Hypic 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a3be6b186805](https://pan.quark.cn/s/a3be6b186805)

---

## 一、提示词在哪个环节输入

AI 扩图的操作链路（多个第三方教程的步骤一致）：

1. 打开 Hypic，导入照片，进入「AI 修图」的编辑界面。
2. 在工具页找到 **AI Expand（AI 扩图）**，必要时先用调整功能把照片裁剪到位。
3. 拖动画面的边界，选择要往外补出来的范围。
4. 在提示词输入框里**用一句话描述你想要补出来的场景**。
5. 点 **Generate（生成）**，等几秒出结果；不满意就再生成一次。

不写提示词直接生成也可以——AI 会顺着画面现有内容自然延伸，只是补出来的内容不受你控制。

## 二、提示词的写作公式

扩图提示词就是在告诉 AI「边界外面是什么」。写之前先定三件事：**场景、光线、氛围**，再按这个顺序组织成一句话：

```text
场景主体 + 光线描述 + 氛围/风格词
```

对照着看几条由笼统到具体的写法：

| 笼统 | 具体 |
| --- | --- |
| `beautiful sunset` | `A calm beach at sunset, warm orange sky with soft clouds, gentle waves reflecting the golden light` |
| `flowers` | `A wildflower meadow under blue skies, soft sunlight, gentle breeze, dreamy pastel colors` |
| `city at night` | `Cinematic city street at night with neon lights and reflections on wet pavement` |

要点：

- **具体胜于笼统**：写「有一片湖和山的日落湖边」比只写「风景」出片得多。
- **一次描述一个场景**：又想要雪山又想要热带沙滩，AI 会两头不靠。
- **加光线与氛围词**：`golden hour`（黄金时刻）、`soft light`（柔光）、`dreamy`（梦幻）、`cinematic`（电影感）这类词对真实感提升明显。
- **生成不满意就换措辞再生成**：同一段提示词每次输出都不同，多试几次是正常用法。

网上流传的现成提示词基本都是英文，直接复制粘贴即可；自己写时把场景描述清楚是第一位的。

## 三、按场景挑现成提示词

以下整理自公开流传的 Hypic 提示词合集，按场景分类，复制即可用。

**自然风光**

```text
Sunset beach with palm trees and golden sky
Green forest with sunlight rays through trees
Snow-covered mountain range with a frozen lake and frost-tipped pine trees
A vast meadow blooms with vibrant flowers under a golden sunset sky
```

**城市与夜景**

```text
Cinematic city street at night with neon lights and reflections
Dramatic sunset highway with golden hour lighting
Japanese city street at night with glowing signs
```

**室内与空间**

```text
Luxury hotel suite with modern furniture and soft lighting
Modern minimalist apartment interior with large windows
Trendy coffee shop background with warm tones
```

**动漫与幻想**

```text
Anime cherry blossom park during spring
Anime countryside with green fields and blue sky
Fantasy forest with mist, glowing fireflies and magical lighting
```

**旅行街景**

```text
Santorini-inspired coastal village with white walls and blue domes
Traditional European street café with cobblestone roads
Mediterranean seaside town in the afternoon sun
```

## 四、两条完整示例

适合直接粘进提示词框的长描述（对画面细节交代更完整，出图更可控）：

```text
A majestic mountain surrounded by fields of golden sunflowers, a serene lake
at its base reflecting the warm hues of a dramatic sunset sky. The background
is blurred for a dreamy, artistic feel.
```

```text
The night sky is filled with bright stars and the Milky Way. Dimly lit tents
nearby host people gathered around a glowing bonfire.
```

把场景换成你照片需要的方向即可：人像照往背景补环境，风景照往外补天空或前景。

## 五、生成失败与提示词无关的情况

- **扩图范围拉得太宽**：画面比例改得太极端时，AI 可能生成不出来或结构崩坏，把边界收一点再试。
- **一直报错转圈**：属于功能故障而非提示词问题，处理办法见 [常见问题排查](常见问题排查.md)。
- **生成次数限制**：AI 功能有免费额度限制的可能，部分生成或高级能力可能提示付费，以应用内实际提示为准（功能入口与规则随版本变化，时效截至 2026-09）。

扩图在整个修图流程里的位置与其他工具的配合，见 [AI修图功能怎么用](AI修图功能怎么用.md)；还没装好的先看 [下载与安装教程](下载与安装教程.md)。
