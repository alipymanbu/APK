# codeformer 手机版与电脑版 CodeFormer 的区别

> 搜 codeformer 时会同时看到手机 App、开源项目、网页 demo 三样东西。本篇讲清它们各是什么、各自适合什么场景。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [修复深浅与保真度怎么调.md](修复深浅与保真度怎么调.md) · [人脸修复操作步骤.md](人脸修复操作步骤.md)

---

> [!IMPORTANT]
> **codeformer 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/94c6ce4474d4](https://pan.quark.cn/s/94c6ce4474d4)

---

## 一、同一个名字，三个东西

| | 手机版 App（本套文档主角） | 开源项目（电脑版） | 官方网页 demo |
| --- | --- | --- | --- |
| 是什么 | 基于开源模型做的安卓修图应用 | 南洋理工大学 S-Lab 团队发布的原始项目（NeurIPS 2022 论文） | 官方挂在网上的演示页 |
| 用起来 | 装 APK 即用，中文界面 | 要配 Python 环境，参数全靠命令行 | 打开网页就能用 |
| 可调的东西 | 一个修复幅度滑块 + 常规编辑工具 | 保真度数值、背景增强、人脸放大、批量文件夹、视频逐帧 | 保真度数值滑杆、背景增强等开关 |
| 素材来源 | 手机相册、手机里的视频 | 电脑上的图片 / 视频 / 整个文件夹 | 只能传图片 |
| 费用 | 免费 | 免费开源 | 免费 |

开源项目的源码在 [github.com/sczhou/CodeFormer](https://github.com/sczhou/CodeFormer)，手机版 app 正是基于它做的。

## 二、各自适合什么人

- **手机版**：想直接在手机上修相册照片、不想折腾电脑环境。决定用手机版的话，[codeformer 安装文件资源（夸克网盘）](https://pan.quark.cn/s/94c6ce4474d4)拿去安装即可。
- **电脑版**：要批量处理整个文件夹、处理视频，或者想把背景增强这类开关也调上。门槛在环境配置，而且速度依赖显卡——有博主实测带 4GB 显存的 NVIDIA 显卡就够用，没有 N 卡用 CPU 跑会非常慢。
- **网页 demo**：只想先试一两张的效果，不想装任何东西。

## 三、在线 demo 认准官方渠道

官方的网页 demo 有三个，都在开源仓库里列着：

- [huggingface.co/spaces/sczhou/CodeFormer](https://huggingface.co/spaces/sczhou/CodeFormer)
- [replicate.com/sczhou/codeformer](https://replicate.com/sczhou/codeformer)
- [openxlab.org.cn/apps/detail/ShangchenZhou/CodeFormer](https://openxlab.org.cn/apps/detail/ShangchenZhou/CodeFormer)

两点提醒：demo 的结果页是限时保存的，处理完要尽快下载；搜索引擎里还有不少套用 CodeFormer 之名的第三方网站，有第三方资料提到开发者提醒过这类未授权站点，稳妥起见只认上面仓库里列出的渠道。

## 四、许可上的差别

开源模型的许可是 S-Lab License 1.0，对再分发和商业使用有约束，细节以其仓库里的 LICENSE 文件为准。手机版 app 是第三方团队打包的独立产品，它的使用条款以应用内说明为准，与开源许可不是一回事。

选定手机版之后，从[下载与安装教程.md](下载与安装教程.md)开始；修复幅度怎么调见[修复深浅与保真度怎么调.md](修复深浅与保真度怎么调.md)。
