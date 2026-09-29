# Markor 笔记格式支持与 PDF 导出

> 本篇讲 Markor 能写哪些格式、Markdown 编辑时哪些辅助功能最常用、预览怎么用，以及怎么把笔记变成 PDF 或 HTML 带走。
> **相关文档**：[使用教程.md](使用教程.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **Markor 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ba4e76fb6fb8](https://pan.quark.cn/s/ba4e76fb6fb8)

---

## 一、能打开哪些格式的文件

在 Notebook 里新建文件时可以选格式，日常以这几类为主：

| 格式 | 编辑体验 | 预览 |
| --- | --- | --- |
| Markdown | 高亮 + 格式按钮 | 完整渲染 |
| todo.txt | 高亮 + 任务按钮 | 列表视图 |
| 纯文本 / 代码（json、yaml、toml、ini、csv 等） | 键值高亮 | csv 可渲染成表格 |
| Zim Wiki | 高亮 + 链接支持 | 渲染 |
| AsciiDoc | 高亮 + 格式按钮 | 渲染 |
| Org-Mode | 高亮 + 格式按钮 | 无独立预览 |

不同格式的能力是逐步迭代进来的，你手上这份 2.16.0 已包含上述全部；个别格式的细节（比如 Org-Mode 只在编辑侧提供便利、没有独立预览）以官方 README 与 NEWS 页说明为准：[https://github.com/gsantner/markor/blob/master/README.md](https://github.com/gsantner/markor/blob/master/README.md)。

## 二、Markdown 编辑最常用的几个辅助

- **格式按钮**：底部一排按钮覆盖加粗、斜体、标题、列表、表格、代码块等，选中文字再点按钮，会直接包上对应标记；
- **插图片**：点图片按钮，可以从相册选或现拍，Markor 会把图片复制到笔记旁边并把链接写进文本。注意链接建议写本地相对路径，换设备时把整个文件夹一起拷走才不丢图；和 Obsidian 等桌面笔记共用目录时，路径约定更要注意，见 [与Obsidian共用笔记目录.md](与Obsidian共用笔记目录.md)；
- **目录**：笔记写长了，预览时用目录（TOC）跳章节，编辑侧也能快速定位；
- **YAML 头部**：文件开头写 `---` 包起来的键值块，预览时可被识别（键的显示可在 Markdown 设置里配置），适合做笔记元信息（标题、日期、标签）；
- **提示框**：用 `!!! note` 这类 Admonition 语法可以把一段内容框成带颜色标题的提示块，预览时生效（2.9 起支持），整理「注意/警告」类内容很好用；
- **数学公式**：用 KaTeX 语法写公式，预览时渲染成数学排版，记理工科笔记用得上；
- **图表与演示**：官方更新日志还提到 Mermaid 图表渲染与 Markdown 演示播放相关能力（2.2/2.9 版前后加入），写法和入口以官方 NEWS 页示例为准：[https://github.com/gsantner/markor/blob/master/NEWS.md](https://github.com/gsantner/markor/blob/master/NEWS.md)。

Markdown 语法本身是 CommonMark 规范，不熟的话可以看官方推荐的入门教程：[https://commonmark.org/help/tutorial/](https://commonmark.org/help/tutorial/)（10 分钟上手）。

## 三、预览、导出 PDF 与 HTML

写完笔记想带走或给别人看，有三条路：

1. **预览模式**：最直接，切到预览看排版效果；
2. **导出 PDF**：预览界面用分享/导出菜单，把当前笔记转成 PDF 存到手机或直接发给别的应用；
3. **导出 HTML**：同样在菜单里，转成网页文件分享出去，对方用浏览器打开。

PDF 和 HTML 导出都从当前渲染结果来，所以导出前先在预览里确认一遍排版，尤其是表格和图片的位置。

## 四、写长文件的两个小工具

- **行号**：顶部文件菜单里勾选开启，编辑和代码块的预览都会显示行号，对照行号讨论内容或排错很方便；
- **片段/模板（snippets）**：把常用文字块存成模板文件，编辑时光标停在要插入的位置，点底部片段按钮选模板即插入；新建文件时也能直接按模板创建。

格式写好之后，笔记一般要长期保存或多设备使用，接下来看 [使用教程.md](使用教程.md) 里 Notebook 的组织方式，或 [文件同步与备份加密.md](文件同步与备份加密.md) 里的同步与备份方案。
