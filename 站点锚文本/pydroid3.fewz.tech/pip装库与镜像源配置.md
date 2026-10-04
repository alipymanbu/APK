# pip装库与镜像源配置

> pydroid3 里装第三方库有两条路：菜单里的 Pip 界面和终端命令，装到的是同一套环境。本篇讲怎么装、重型库为什么装得快，以及装不动时的排查顺序。
> **相关文档**：[手机上写Python的环境怎么搭.md](手机上写Python的环境怎么搭.md) · [图形界面与matplotlib画图.md](图形界面与matplotlib画图.md) · [常见报错与解决办法.md](常见报错与解决办法.md)

## 一、两条装库的路

- **Pip 界面**：抽屉菜单里的 Pip 入口，输入包名点安装，适合不想敲命令的时候；
- **终端命令**：切到 Terminal 标签，直接敲 `pip install 包名`。

两种方式装到的是同一套环境，混用没有问题。装完在解释器里 `import` 一下验证：

```python
import numpy
print(numpy.__version__)
```

## 二、重型库为什么装得快

numpy、scipy、matplotlib、scikit-learn、jupyter 这类含原生代码的科学计算库，在手机上从 PyPI 源码编译既慢又容易失败。pydroid3 对这类库提供预编译 wheel 仓库，`pip install` 时自动优先取预编译包，这是它相对 Termux 手动配环境最大的省心之处。

常用库都能直接装：

```bash
pip install numpy
pip install pandas
pip install matplotlib
pip install requests
pip install beautifulsoup4
pip install flask
```

## 三、repository plugin 别主动装

应用商店里有一个独立的 Pydroid repository plugin（包名 `ru.iiec.pydroid3.quickinstallrepo`），存放含原生库的预编译包。开发者说明写得很明确：**除非应用要求，否则不要单独安装**——pydroid3 需要时会弹提示，照提示装上即可。

设备上装不了这个插件，还有一条替代路：在设置里取消「使用预编译库仓库」，让库从源码编译。这条路耗时长、可能要手动装依赖，只有别无选择时再用。

## 四、装不动时的排查顺序

| 现象 | 先做的事 |
| --- | --- |
| 下载慢、超时 | 挂国内镜像源（见下） |
| `No matching distribution found` | PyPI 上可能没有适配 Android 或该架构的包，换个包名搜替代品；科学计算类库覆盖最全，系统相关的库移植优先级低 |
| 空间报错 | 清内置存储，至少留 250 MB，重型库要更多 |
| 装上了却 `ModuleNotFoundError` | 见[常见报错与解决办法.md](常见报错与解决办法.md) |

国内网络环境下载慢，临时挂镜像源：

```bash
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```

把 `numpy` 换成要装的包名即可，镜像地址以对应站点当前公告为准。

## 五、装库前确认的两件事

- **架构**：预编译包按 CPU 架构分发，架构不对会找不到可用的包——这也是安装环节就要核对的事，环境怎么选与架构判断见[手机上写Python的环境怎么搭.md](手机上写Python的环境怎么搭.md)；
- **空间**：scipy、jupyter 这类库装完动辄几百 MB，装之前先看剩余空间，避免装到一半失败留下半成品。

装好库之后能干什么？把 Flask 跑起来把手机当小型 Web 服务器、用 matplotlib 画图，都整理在 [pydroid3 介绍站](https://pydroid3.fewz.tech/)上。
