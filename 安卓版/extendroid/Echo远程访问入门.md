# Extendroid Echo 远程访问入门

> Echo 是 Extendroid 的远程访问功能（beta）：在任意浏览器里登录，就能操作你设备上的应用。本篇讲它能做什么、怎么开始、连接怎么计费。
> **相关文档**：[多窗口使用方法与手势操作.md](多窗口使用方法与手势操作.md) · [常见问题排查.md](常见问题排查.md)

---

Extendroid Echo 是官方提供的配套远程功能（beta 阶段）：设备上的应用窗口可以直接跑在浏览器标签页里，人在外面也能用。它和 [息屏保活与后台运行设置.md](息屏保活与后台运行设置.md) 组合起来，就是「设备放家里挂着任务，人在外面通过浏览器接管」的用法。

## 一、Echo 能做什么

按官方页面的说法，Echo 提供这几类能力：

| 场景 | 说明 |
| --- | --- |
| 浏览器里跑应用 | 把手机上已开的应用窗口显示在任意浏览器的标签页里 |
| 远程管理设备 | 解锁设备、查看通知、管理文件 |
| 当外接显示 | 用平板、电脑等任何带浏览器的设备充当安卓设备的外接屏幕 |
| 分享部分应用 | 安全地把某几个应用的使用权限交给别人，不影响你自己手上的操作 |

Echo 处于 beta 阶段，功能边界以官方页面为准。

## 二、怎么开始

1. 在手机上把 Extendroid 设置完成（Shizuku 就绪、能正常开窗口，见 [多窗口使用方法与手势操作.md](多窗口使用方法与手势操作.md)）；
2. 打开官网 Echo 页：[extendroid.pages.dev/echo](https://extendroid.pages.dev/echo)；
3. 登录后页面会显示你的设备与设备上的应用，点开即可在浏览器里使用。

## 三、连接怎么计费：Boosters

Echo 的连接消耗一种叫 **Boosters** 的额度。按官方计费页（[extendroid.pages.dev/boosters](https://extendroid.pages.dev/boosters)）的口径：

| 情形 | 消耗 |
| --- | --- |
| 每次建立连接 | 至少扣 0.05 Boosters（该数值标注为可能调整） |
| 纯本地 P2P 直连 | 除上述最低消耗外不再追加 |
| 连接部分或全部经过中继服务器 | 按依赖程度追加扣费 |

几点补充，均来自官方页面，随时可能变化、以官方页面为准：

- 官方称连接全程端到端加密，走中继也不会降低加密强度；
- 早期测试阶段，按要求提交有效问题反馈可获最多 500 Boosters 奖励；
- 购买入口与套餐也设在计费页。

## 四、想在大屏上用：官方 Windows 客户端

除了浏览器里的 Echo，官方还有一个独立的 Windows 客户端 **Extendroid-Win**，定位是把安卓设备「延伸」到电脑上使用。安装包在它的 GitHub 发布页：[github.com/legendsayantan/Extendroid-Win/releases](https://github.com/legendsayantan/Extendroid-Win/releases)，官网 [extendroid.pages.dev](https://extendroid.pages.dev) 的 Windows 按钮也指向这里。它与手机端 Extendroid 配套，功能边界与更新以发布页为准。

## 五、安全提醒

Echo 等于把设备的远程控制权挂到了网上，使用时建议：

- 只在自己的可信设备上登录 Echo；
- 用完退出登录，别让公共电脑记住会话；
- 不把账号借给他人 —— 分享应用请走 Echo 自带的「分享部分应用」能力，而不是交出账号。
