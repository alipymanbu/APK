# CatShare 开启 Shizuku 发送教程

> 本文讲为什么 CatShare 发送文件要先配 MAC 地址，以及用 Shizuku、手动输入、系统权限三种方案配出来的完整步骤。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [连接失败排查方法.md](连接失败排查方法.md) · [互传联盟是什么意思.md](互传联盟是什么意思.md)

---

> [!IMPORTANT]
> **CatShare 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/5a097b31e285](https://pan.quark.cn/s/5a097b31e285)

---

## 一、先搞清楚：为什么发送要折腾这一步

互传联盟协议把设备的 MAC 地址当认证信息的一部分——发起传输的一方得报出自己 WiFi 直连接口（`p2p0`）的 MAC 地址。而从安卓系统收紧权限开始，普通应用已经拿不到这类无法重置的序列号，所以 CatShare 给你留了三条获取途径：**手动配置、系统权限、Shizuku / Root**，按这个优先级尝试。

一个能省事的判断：**你只是收文件的话，这一整篇都不用管**——接收不需要 MAC 地址，先看 [接收文件操作步骤.md](接收文件操作步骤.md)。只有你要当发送方、而且发送时提示 MAC 相关问题，才需要往下走。

## 二、方案一：用 Shizuku 自动获取（推荐）

Shizuku 的作用是给应用临时提升权限，让 CatShare 自己把 MAC 读出来，不用你手算。步骤：

1. 安装 CatShare 和 Shizuku 两个应用（Shizuku 从其官网或应用商店获取）。
2. 启动 Shizuku。你的系统是 Android 11 及以上，可以在 Shizuku 里选「无线调试」方式启动：进手机的「开发者选项」→「无线调试」→ 在 Shizuku 里按引导完成配对，回到 Shizuku 看到「已启动」即可。系统低于 Android 11 则需要连电脑用 adb 启动，命令以 Shizuku 界面给出的为准。
3. 打开 CatShare，此时会弹出 Shizuku 的授权请求，选择允许（对应 `moe.shizuku.manager.permission.API_V23` 权限）。
4. 回到 CatShare 主界面发起发送，此时它会通过 Shizuku 自动取得 MAC 地址。

注意事项：

- **Shizuku 必须保持运行**。手机重启后 Shizuku 会停，要重新按第 2 步启动一次，CatShare 才能继续自动获取。
- 各版本界面文案略有差异，开关位置以应用内实际显示为准。

## 三、方案二：手动输入 MAC 地址

不想装 Shizuku，就自己查一次填进去。这需要你手头有电脑和 adb：

```shell
adb shell ip addr show p2p0
```

输出里 `link/ether` 后面那一串就是 `p2p0` 接口的 MAC 地址，把它填进 CatShare 设置里的手动配置项。

两个坑必须注意：

1. **设备待机时和发起传输时的 MAC 可能不一样**——官方 README 特意标了这条，抓取时要以发起传输那一刻的值为准。也就是说，手动方案更适合 MAC 稳定的机型，配完最好立刻试发一次。
2. **部分 Samsung 设备的 WiFi Direct MAC 地址会周期性变动，这种情况下手动配置无效**——这类机型直接换 Shizuku 方案。

## 四、方案三：系统权限 / Root

- **系统权限**：如果应用拿到了 `android.permission.LOCAL_MAC_ADDRESS` 权限（典型情况是应用由系统签名预装），CatShare 会自动读到 MAC，什么都不用配。普通用户自己装的包基本遇不到这条路，知道有这回事即可。
- **Root**：和 Shizuku 同属「提权自动获取」，Root 后同样能自动读取。有 Root 的机器用 Shizuku 跑起来更省事，二选一即可。

## 五、三个方案怎么选

| 方案 | 要准备什么 | 适合谁 | 失效场景 |
| --- | --- | --- | --- |
| Shizuku | 装 Shizuku 并按引导启动 | 大多数人 | 重启后 Shizuku 停了要重新启动 |
| 手动输入 | 电脑 + adb 查一次 | 不想装 Shizuku、MAC 稳定的机型 | Samsung 等 MAC 周期性变动的机型无效 |
| 系统权限 | 系统签名预装 | 普通用户基本碰不到 | 不适用 |

配完发送仍失败的，按 [连接失败排查方法.md](连接失败排查方法.md) 逐条排查。
