# LNReader 自定义 CSS 调整阅读排版

> 想改正文缩进、字体颜色、隐藏滚动条这类界面细节，CSS 就能解决；本篇的示例可以直接抄。
> **相关文档**：[阅读界面设置与自定义.md](阅读界面设置与自定义.md) · [书源插件安装教程.md](书源插件安装教程.md) · [常见问题排查.md](常见问题排查.md)

---

> [!IMPORTANT]
> **LNReader 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/2e92275313cf](https://pan.quark.cn/s/2e92275313cf)

---

## 一、入口在哪

进「设置 → Reader → Advanced」，里面有两个标签页：**CSS** 和 **JS**。每个页面提供三个动作：

- **Import**：从手机文件里导入写好的代码；
- **Reset**：清空当前代码；
- **Save**：保存。

注意：改动保存后，**重新打开章节才会生效**，正在看的这章不会热更新。

## 二、你的 CSS 能覆盖什么

你的样式会以内联 `<style>` 块追加在默认样式之后，所以能覆盖内置主题的任何规则。改正文排版主要盯这几个选择器：

| 选择器 | 对应的部分 |
| --- | --- |
| `#LNReader-chapter` | 章节正文容器 |
| `#LNReader-chapter p` | 正文段落 |
| `#LNReader-chapter img` | 章节里的插图 |
| `#ToolWrapper` | 侧边滚动条 |
| `.highlight` | TTS 朗读到的高亮块 |
| `body.page-reader` | 翻页模式下整个页面 |

## 三、直接可抄的示例

**正文首行缩进**（中文阅读最常见的诉求）：

```css
#LNReader-chapter p {
  text-indent: 2em;
  margin-bottom: 0.8em;
}
```

**章节标题配色，跟主题走**：

```css
#LNReader-chapter h1,
#LNReader-chapter h2 {
  color: var(--theme-primary);
  border-bottom: 1px solid var(--theme-outline);
  padding-bottom: 0.3em;
}
```

**插图加圆角**：

```css
#LNReader-chapter img {
  border-radius: 8px;
}
```

**隐藏侧边滚动条**：

```css
#ToolWrapper {
  display: none;
}
```

**TTS 高亮换个醒目配色**：

```css
.highlight {
  background-color: var(--theme-primary) !important;
  color: var(--theme-onPrimary) !important;
  border-radius: 3px;
}
```

## 四、用主题变量写，换主题不用重写

阅读设置和主题颜色都以 CSS 变量挂在 `:root` 上，样式里引用变量就能自动跟随你当前的主题：

```css
:root {
  --readerSettings-textSize;   /* 正文字号 */
  --readerSettings-textColor;  /* 正文颜色 */
  --readerSettings-lineHeight; /* 行高 */
  --readerSettings-padding;    /* 左右留白 */
  --theme-primary;             /* 主题主色 */
  --theme-surface;             /* 背景色 */
  --theme-outline;             /* 描边色 */
}
```

写法上不声明、直接 `var(--theme-primary)` 这样引用即可；完整变量表见官方文档 [Custom CSS & JS](https://www.lnreader.app/docs/custom-css-js)。

## 五、JS 那个标签页是干嘛的

Custom JS 在阅读器的 WebView 里执行，内置脚本加载完后运行，能拿到完整 DOM。官方给的典型用法：删掉某些源正文里混进的广告元素、在章首插入字数统计。不写代码的读者用不上它，CSS 那个标签页已经覆盖排版需求。

## 六、有些源自带修复样式

插件作者可以随插件附带 `custom.css` / `custom.js`，装插件时会一起下载，并且**先于**你的全局自定义代码加载——某个源的排版特别乱时，插件作者可能已经修过一轮，不用你动手。

CSS 之外的基础排版与主题设置见[阅读界面设置与自定义.md](阅读界面设置与自定义.md)。
