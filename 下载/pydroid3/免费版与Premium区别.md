# Pydroid 3 免费版与 Premium 的区别

> 本篇讲 Pydroid 3 免费版和 Premium 各自包含什么、哪些功能与库是 Premium 专属、为什么这样划，以及解锁方式。
> **相关文档**：[图形界面与绘图.md](图形界面与绘图.md) · [pip安装第三方库.md](pip安装第三方库.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **Pydroid 3 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/c31d7b093aff](https://pan.quark.cn/s/c31d7b093aff)

---

## 一、免费版里有什么

官方功能列表里不带星号的都在免费版里，核心的这些够你完成大部分 Python 学习与脚本任务：

- 离线 Python 解释器（不需要联网跑程序）；
- pip 与预编译库仓库（numpy、scipy、matplotlib、scikit-learn、jupyter 等）；
- 内置 C / C++ / Fortran 编译器、Cython 支持；
- Tkinter、Kivy、PySide6、pygame 等 GUI 方案；
- 完整终端模拟器；
- PDB 断点调试；
- 自带示例。

免费版含广告。

## 二、Premium 才有的部分

官方列表中标星的功能与库：

| 类别 | 内容 |
| --- | --- |
| 编辑器 | 代码预测、自动缩进、实时代码分析 |
| 视觉/机器学习 | OpenCV（要求设备支持 Camera2 API）、TensorFlow、PyTorch |

日常写脚本、学语法、做常规画图与 GUI，用不到这几项；只有代码补全体验和机器学习方向会明显碰到这道线。

## 三、为什么有的库锁在 Premium 里

官方给过解释：这几类库移植难度极大，是请另一位开发者完成的，按双方约定，其移植的 fork 只提供给 Premium 用户。所以这不是功能阉割，而是移植成本的授权安排——官方同时表示欢迎社区开发这些库的免费 fork。

## 四、怎么解锁

Premium 通过应用内购买解锁（Google Play 上该应用同时标注「含广告」与「应用内购买」）。价格随时可能调整，以应用内购买页当时显示的为准。

另外提醒一句：iOS 的 App Store 上另有一个同名「Pydroid 3」，不是本文说的这个 Android 版（开发者不是同一家），别看混了。

## 五、两边怎么选

- 只在手机上学 Python、写小工具、跑爬虫和数据小实验——免费版覆盖得到，先装了用着，碰到具体缺的功能再决定；
- 需要代码预测与实时代码分析、或明确要跑 OpenCV / TensorFlow / PyTorch——那就是 Premium 的适用范围。

无论哪个版本，安装与环境问题都从[下载与安装教程.md](下载与安装教程.md)开始，装库的细节在[pip安装第三方库.md](pip安装第三方库.md)，界面类功能怎么跑在[图形界面与绘图.md](图形界面与绘图.md)。
