# Pydroid 3 里 pip 装库与环境配置

Pydroid 3 自带 pip，装库不用出应用；含原生代码的重库还有预编译仓库兜底。这篇把装库的两条路、重型库为什么快、镜像源怎么挂、装不动时从哪排查，一次说清。

## 一、两条装库路，装到同一套环境

- **菜单里的 Pip 入口**：输入包名点安装，适合不想敲命令的时候；
- **终端命令**：切到 Terminal 标签，直接敲 `pip install 包名`。

两种方式装到的是同一套环境，混用没有问题。

## 二、重型库优先走预编译仓库

numpy、scipy、matplotlib、scikit-learn、jupyter 这类科学计算库含原生代码，在手机上从 PyPI 源码编译又慢又容易失败。Pydroid 3 为它们准备了预编译 wheel 仓库，`pip install` 时自动优先取预编译包：

```bash
pip install numpy
pip install pandas
pip install matplotlib
```

装完在解释器里 `import` 一下即可验证。GUI 相关的库（Tkinter、Kivy、PySide6、pygame）装好之后怎么跑，属于另一个话题，可先看[运行与调试技巧](运行与调试技巧.md)里的运行模式一节。

## 三、repository plugin：弹提示再装

应用商店渠道有个独立的预编译仓库插件，存放含原生库的预编译包。要注意的是：**别主动装它**——应用需要时会自己弹提示，照提示装上就行。设备装不了插件时，还有一条替代路：在设置里关掉「使用预编译库仓库」，让库从源码编译；这条路耗时长，只有别无选择时再走。

## 四、国内网络环境：挂镜像源

下载慢、超时，临时挂个国内镜像立竿见影：

```bash
pip install 包名 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

把 `包名` 换成要装的库即可，镜像地址以对应站点当前公告为准。

## 五、装不动时的排查顺序

| 现象 | 先做的事 |
| --- | --- |
| 下载慢、超时 | 换国内镜像源 |
| No matching distribution found | 该包可能没有适配 Android 或当前架构的预编译包，搜搜替代品 |
| 装到一半失败 | 清内置存储——应用自身至少留 250 MB，重型库要更多 |
| 装上了却 ModuleNotFoundError | `pip list` 确认真的装上了；装的名字和 import 的名字经常不一样（装 `beautifulsoup4`、import 的是 `bs4`） |

最后一条里还有个容易忽略的架构问题：预编译包按 CPU 架构分发，架构不对会找不到可用的包。架构怎么判断、装错架构会怎样，[pydroid3 资料站](https://pydroid3.gerw.hair/) 的安装步骤里写得很细。

## 六、装库前确认两件事

1. **架构**：知道自己设备的 ABI，预编译包能不能装心里就有数了；
2. **空间**：scipy、jupyter 这类库装完动辄几百 MB，先看剩余空间，避免装到一半失败留下半成品。

确认完这两件，剩下的交给 pip 就好。
