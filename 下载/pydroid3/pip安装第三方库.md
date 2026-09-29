# Pydroid 3 用 pip 安装第三方库

> 本篇讲 Pydroid 3 里怎么装第三方库：pip 怎么用、重型库为什么装得快、repository plugin 是什么、装不动时从哪里排查。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [图形界面与绘图.md](图形界面与绘图.md) · [常见问题与报错解决.md](常见问题与报错解决.md)

---

> [!IMPORTANT]
> **Pydroid 3 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/c31d7b093aff](https://pan.quark.cn/s/c31d7b093aff)

---

## 一、pip 就在应用里

Pydroid 3 自带 pip，装库有两条路：

- **菜单里的 Pip 入口**：输入包名点安装，适合不想敲命令的时候（界面入口位置以你手上的版本为准）；
- **终端命令**：切到 Terminal 标签，直接敲命令。

两种方式装到的是同一套环境，混用没有问题。

## 二、重型库优先用它的预编译仓库

numpy、scipy、matplotlib、scikit-learn、jupyter 这类含原生代码的科学计算库，在手机上从 PyPI 源码编译既慢又容易失败。Pydroid 3 对这类库提供预编译 wheel 仓库，`pip install` 时会自动优先取预编译包，免去漫长的源码编译。

常见的都能直接装：

```bash
pip install numpy
pip install pandas
pip install matplotlib
pip install requests
pip install beautifulsoup4
pip install flask
```

装完在解释器里 `import` 一下验证：

```python
import numpy
print(numpy.__version__)
```

装完能干什么？把 Flask 跑起来、把手机当 Web 服务器用，见[手机上跑Web服务.md](手机上跑Web服务.md)。

## 三、Pydroid repository plugin：别主动装，弹提示再装

Google Play 上有一个独立的 **Pydroid repository plugin**（包名 `ru.iiec.pydroid3.quickinstallrepo`），它存放含原生库的预编译包。官方说明明确写着「除非应用要求，否则不要单独安装」——它存在的目的是满足应用商店对下载可执行代码的政策，Pydroid 3 需要时会提示你安装，照提示装上即可。

如果设备上装不了这个插件，还有一条替代路：在 Pydroid 3 的设置里取消「使用预编译库仓库」（use prebuilt libraries repository）选项，让库从源码编译。这条路耗时长，而且可能要手动装依赖，只有别无选择时再用。

## 四、源码编译有内置编译器兜底

Pydroid 3 内置了 C / C++ / Fortran 编译器，还支持 Cython。pip 里没有现成预编译包的库，你仍然可以尝试从源码构建——能不能编译成功取决于库本身，但至少工具链是齐的，不必直接放弃。

## 五、装不动时的四步排查

| 现象 | 先做的事 |
| --- | --- |
| 下载慢、超时 | 换国内镜像源（见下） |
| `No matching distribution found` | 该包可能没有适配 Android 或该 CPU 架构的预编译包，换个包名搜搜有没有替代品；系统相关的库官方移植优先级低，科学计算类库覆盖最全 |
| 空间报错 | 清内置存储，至少留 250 MB，重型库要更多 |
| 装上了却 `ModuleNotFoundError` | 见[常见问题与报错解决.md](常见问题与报错解决.md) |

国内网络环境下下载慢，可以临时挂镜像源：

```bash
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```

把 `numpy` 换成你要装的包名即可。镜像地址以对应站点当前公告为准。

## 六、装库前确认的两件事

- **架构**：预编译包按 CPU 架构分发，架构不对会找不到可用的包。网盘里这份安装包是 x86_64 构建，架构问题见[下载与安装教程.md](下载与安装教程.md)。
- **空间**：scipy、jupyter 这类库装完动辄几百 MB，装之前先看剩余空间，避免装到一半失败留下半成品。

GUI 相关的库（Tkinter、Kivy、PySide6、matplotlib 画图）装好之后怎么跑起来，见[图形界面与绘图.md](图形界面与绘图.md)。
