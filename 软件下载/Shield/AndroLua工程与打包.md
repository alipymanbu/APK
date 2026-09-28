# AndroLua 工程与打包入门

> Shield 里打开的工程属于 AndroLua+ 体系。本篇讲这套体系的来历、工程里每个文件的角色，以及写好的脚本怎么变成一个 APK。
> **相关文档**：[编辑器与工程结构.md](编辑器与工程结构.md) · [市场与文件类型.md](市场与文件类型.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **Shield 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/1ba1905718c4](https://pan.quark.cn/s/1ba1905718c4)

---

## 一、这套工程是哪来的

你在 Shield 编辑器里打开的那套工程（main.lua + layout.aly + init.lua）来自 AndroLua+ 生态。AndroLua+ 是在安卓手机上用 Lua 语言写安卓程序的工具：可以直接调用 Java API、编写界面程序，还能把写好的 Lua 工程打包成 APK 安装文件。官方站点是 [androlua.com](http://androlua.com/)，开源项目在 [github.com/nirenr/AndroLua_pro](https://github.com/nirenr/AndroLua_pro)，语法入门与帮助文档从这两处入手最稳。

Shield 市场里数量最多的 ALP 文件，标注的正是「AndroLua+工程文件」——两者怎么接上，见 [市场与文件类型.md](市场与文件类型.md)。

## 二、脚本开头那几行是什么

AndroLua+ 体系的脚本，开头几乎都长这样：

```lua
require "import"
import "android.app.*"
import "android.widget.*"
```

- `require "import"`：载入 import 模块，之后才能用 `import` 的简写方式；
- `import "android.app.*"`：按包导入安卓的类，星号表示整包；也可以精确到单个类，如 `import "android.widget.Button"`。

常用内置模块还有 http、cjson、socket、xml、zlib 等，用到哪个就在脚本里导入哪个。

## 三、工程文件各自管什么

| 文件 | 角色 |
| --- | --- |
| main.lua | 程序入口，运行与打包都从它开始 |
| init.lua | 工程信息：应用名（appname）、版本（appver）、包名（packagename）写在这里 |
| layout.aly | 布局表文件，界面长什么样由它描述，配合 loadlayout 载入 |
| java/ | 放 Java 相关内容的目录 |
| icon.png / welcome.png | 放进工程目录可分别替换应用图标与启动图 |

## 四、从工程到 APK

AndroLua+ 的打包规则很直接：在脚本目录放一个 init.lua，写上这三行，就能把目录下所有 lua 文件打包，main.lua 是程序入口：

```lua
appname = "demo"
appver = "1.0"
packagename = "com.androlua.demo"
```

打出来的 APK 用的是 debug 签名，自己装机测试够用。打包入口在 Shield 界面的哪个位置，以你手上的版本实际显示为准——这一节讲的是 AndroLua+ 体系本身的规则。

改脚本改出报错时，先按 [常见问题与排查.md](常见问题与排查.md) 的思路定位行号，再回头检查 layout.aly 里对应的控件写法。
