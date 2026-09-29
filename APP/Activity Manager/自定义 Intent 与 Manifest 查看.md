# 自定义 Intent 与 Manifest 查看

> 本篇讲 Activity Manager 的两个进阶入口：用 Intent Builder 拼一条带参数的启动指令，用 Manifest 查看器翻应用的清单找组件名。
> **相关文档**：[启动隐藏 Activity 的方法.md](启动隐藏%20Activity%20的方法.md) · [常见问题与故障排查.md](常见问题与故障排查.md)

---

> [!IMPORTANT]
> **Activity Manager 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/dc74ec1165ee](https://pan.quark.cn/s/dc74ec1165ee)

---

## 一、这两个入口适合谁

- **Intent Builder**：直接点某个 Activity 打不开（通常是缺参数）时，你自己拼 action、数据、附加参数再启动。
- **Manifest 查看器**：想搞清一个应用里有哪些页面、服务、广播接收器，或开发者文档里提到某个组件名却不知道长什么样时，翻它的清单找答案。

说明：这两个入口在较新的官方版本里都有；你手上若是 5.4.11 这类老版本而界面里找不到对应菜单，说明该版尚未加入，更新渠道见 [下载与安装教程.md](下载与安装教程.md)。

## 二、用 Intent Builder 拼一条启动指令

1. 打开 Intent Builder（在应用或 Activity 的菜单里）。
2. 按已知信息填：Action（动作，如 `android.intent.action.VIEW`）、Data（数据地址，常见是网址）、MIME 类型、Extras（附加参数，键值对）。
3. 填完先直接启动试一次；通了再保存下来，下次一键复用。
4. 参数从哪来：目标应用的说明文档、开发者给的调试说明，或在 Manifest 查看器里找 intent-filter 声明的 action。

两个提醒：填错的参数不会有明确报错，只会「点了没动静」，逐项增减着试是唯一办法；让应用打开特定链接、触发它定义过的某个广播，是最常见的两类用法。

## 三、用 Manifest 查看器找组件名

1. 在应用列表里选目标应用，打开 Manifest 查看器。
2. 重点看三类声明：activity（页面入口）、service（后台服务）、receiver（广播接收器），每条都带完整类名。
3. 类名可以直接复制。做免参数直启，拿 activity 的完整类名就够了；要拼 intent，找 intent-filter 里写的 action 与 data 规则。
4. 新版本还支持把拼好的 intent 导出成 URI 或 shell 命令（shell 命令导出是 5.5.0 加的），想在电脑 adb 里复用同一条指令时用得上。

## 四、什么时候该停下来

翻清单、拼 intent 影响的只是「你自己的设备上启动哪个界面」。遇到需要账号密码、需要系统签名权限的组件，普通方式拿不到就到此为止，不要绕。
