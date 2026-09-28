# MMRL 和 Magisk、KernelSU 自带管理器的区别

> MMRL 会不会替代 Magisk 管理器、和 root 管理器是不是冲突、该在哪个界面操作。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [添加模块仓库与安装模块教程.md](添加模块仓库与安装模块教程.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **MMRL 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/e06fce36d68f](https://pan.quark.cn/s/e06fce36d68f)

---

## 一、结论先行：不是替代，是分工

MMRL 和 Magisk 管理器、KernelSU 管理器不冲突，也不互相替代 —— 有公开教程明确写了「MMRL 不取代 Magisk 管理器，而是与它并行」，MMRL 主要管仓库、批量安装和多 root 方案支持。三者的分工是：

| 你要做的事 | 用哪个 |
| --- | --- |
| 刷入 / 升级 root、管理 root 授权、Zygisk 等内核级选项 | Magisk / KernelSU / APatch 自带管理器 |
| 找模块来源、浏览多个仓库、批量装模块、看依赖 | MMRL |
| 模块的启用 / 禁用 / 卸载 | 两边都能操作（见下节） |

MMRL 本身不做 root，也不碰你的 root 授权配置；把理解成「装在 root 之上的模块商店 + 安装器」就对了。

## 二、两个界面操作的是同一批模块

模块文件存放在 root 方案的同一个模块目录里（KernelSU 官方文档标注为 `/data/adb/modules`，Magisk 同样在 `/data/adb` 下），你在 MMRL 里装的模块，回到 Magisk 或 KernelSU 管理器的模块列表里也能看到；反过来，在原管理器里禁用的模块，MMRL 这边显示的状态也是同一个。所以：

- 不需要在两个界面里「同步」或「导入」，没有这一步；
- 同一个模块**别两边同时点操作**，按一个界面的结果为准，避免状态看混。

## 三、同时装了 Magisk 和 KernelSU 才需要小心

只用一种 root 方案的话，上面这些就够了。若手机上同时存在两套（少见但有人这么折腾），KernelSU 官方常见问题明确提示：**KernelSU 的模块系统与 Magisk 的 magic mount 有冲突 —— 只要 KernelSU 中启用了任何模块，整个 Magisk 就不工作了**；只用 KernelSU 的 `su`、不用它的模块，则两者可以共存（KernelSU 改内核、Magisk 改 ramdisk）。官方原文见 [kernelsu.org 的常见问题](https://kernelsu.org/zh_CN/guide/faq)。

这种环境下用 MMRL，先想清楚模块装给哪一套：MMRL 首次启动选的 root 管理器决定了它往哪边装（选错的排查见[常见问题与故障排查.md](常见问题与故障排查.md)第一节）。

## 四、怎么选

- 只是想找模块、批量装、看依赖和反功能标记 → 留在 MMRL 里操作（用法见[添加模块仓库与安装模块教程.md](添加模块仓库与安装模块教程.md)）。
- 要动 root 本身（升级 Magisk、开 Zygisk）→ 回原管理器，那些 MMRL 没有。
- 还没装 MMRL → 按[下载与安装教程.md](下载与安装教程.md)装一份；安装包也可以直接从 [MMRL 安装文件资源（夸克网盘）](https://pan.quark.cn/s/e06fce36d68f) 获取。
