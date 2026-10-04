# 图形界面与matplotlib画图

> pydroid3 里各种「有画面」的程序怎么跑：Tkinter 小窗口、Kivy 与 PySide6 应用、pygame 小游戏、matplotlib 画图，以及中文变方块这个经典问题的解法。
> **相关文档**：[pip装库与镜像源配置.md](pip装库与镜像源配置.md) · [常见报错与解决办法.md](常见报错与解决办法.md)

## 一、Tkinter：最快出窗口的路

Tkinter 是标准库，pydroid3 完整支持，装都不用装：

```python
import tkinter as tk

root = tk.Tk()
root.title("hello")
tk.Label(root, text="在手机上跑起来的窗口").pack(padx=40, pady=20)
root.mainloop()
```

保存后直接运行即可。如果 import 了 tkinter 还是按终端模式跑，在文件第一行加 `#Pydroid run tkinter` 强制指定。

## 二、Kivy 与 PySide6

两个框架都要先装（装库方法见[pip装库与镜像源配置.md](pip装库与镜像源配置.md)）：

- **Kivy**：pydroid3 为它配了 SDL2 后端，装好后运行一个 `import kivy` 的脚本，会自动按 Kivy 模式启动；
- **PySide6（Qt）**：在预编译仓库里提供；matplotlib 的 PySide6 后端也做了适配，画图不需要额外代码。

运行模式也可以用魔法注释强制指定：`#Pydroid run kivy`、`#Pydroid run qt`、`#Pydroid run sdl2`、`#Pydroid run pygame`。

## 三、pygame 小游戏

pygame 2 可用，写法与桌面端一致，`import pygame` 即会按对应模式运行。注意手机屏幕方向、触控输入与桌面键盘的差异，改内置示例比从零写省事。

## 四、matplotlib 画图

装好 matplotlib 后，普通脚本默认按 GUI 模式运行，图会直接弹出来：

```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4]
y = [1, 4, 9, 16]
plt.plot(x, y)
plt.show()
```

两个常撞的点：

- 图弹出后程序像「卡住」——那是 `plt.show()` 的窗口在等关闭，正常现象；
- 只想要终端输出、不想弹图——文件里加 `#Pydroid run terminal`。

## 五、中文显示成方块怎么办

matplotlib 默认字体不含中文字形，中文标签全变成「豆腐块」。桌面上的惯用解法（设 SimHei）在手机上不管用，因为设备里没有这个字体。可行做法：找一个带中文的 `.ttf` 字体文件放进公共目录，画图前注册并指定：

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

font_manager.fontManager.addfont('/sdcard/Documents/Pydroid3/某中文字体.ttf')
plt.rcParams['font.sans-serif'] = ['该字体的名称']
plt.rcParams['axes.unicode_minus'] = False  # 负号正常显示
```

`font.sans-serif` 里填字体注册后的名称（不是文件名），可以先 `print(font_manager.fontManager.ttflist)` 对照。

## 六、哪些图形库要付费

OpenCV（且要求设备支持 Camera2 API）、TensorFlow、PyTorch 这三个库只对 Premium 用户开放，做机器学习、计算机视觉方向才会碰到；纯画图与常规 GUI 用基础版本就够。环境与版本划分的整体说明见 [pydroid3 介绍站](https://pydroid3.fewz.tech/)，跑不起来时先翻[常见报错与解决办法.md](常见报错与解决办法.md)。
