# DFU 升级包制作教程（nrfutil 打包）

> 手机端 DFU 只认打好的升级包，而固件工程师手里通常是一份 `.hex`。本篇讲怎么在电脑上用 Nordic 官方命令行工具 nrfutil 把固件打成能无线升级的 `.zip`，以及参数填错时的典型表现。
> **相关文档**：[DFU固件无线升级教程.md](DFU固件无线升级教程.md) · [常见问题与使用技巧.md](常见问题与使用技巧.md)

---

> [!IMPORTANT]
> **nrfconnect 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/69f8aa2d66a0](https://pan.quark.cn/s/69f8aa2d66a0)

---

## 一、打包这一步为什么在电脑上做

手机上的 nRF Connect 是「发货端」：它把现成的升级包通过蓝牙传给设备。但升级包本身（`.zip`，内含固件映像、init packet 和清单文件）要在电脑上用 Nordic 官方工具 **nrfutil** 生成——它不是这款应用的内置功能。如果你是普通用户、设备商直接给了 `.zip`，跳过本篇；如果你拿到的是 `.hex`，往下看。

## 二、先装 nrfutil，并避开一个新版的坑

nrfutil 从 [Nordic 官方工具页](https://www.nordicsemi.com/Products/Development-tools/nrf-util) 下载，Windows/macOS/Linux 都有。版本分两代，打包命令的入口不一样：

| 版本 | 形态 | 打包命令入口 |
| --- | --- | --- |
| 6.x 及更早 | Python 包（`pip install nrfutil`） | `nrfutil pkg generate …` 直接可用 |
| 7.0 起 | 独立二进制 | 打包子命令**默认未安装**，要先装扩展 |

新版的坑：直接敲 `nrfutil pkg generate` 会报 `Subcommand nrfutil-pkg.exe not found`。解法是把 nRF5 SDK 工具扩展装上，再通过它调用：

```bat
nrfutil install nrf5sdk-tools
nrfutil nrf5sdk-tools pkg generate --help
```

`--help` 能列出完整参数就说明装好了。

## 三、打包命令与参数怎么填

以给 nRF52 设备刷应用固件为例：

```bat
nrfutil nrf5sdk-tools pkg generate --hw-version 52 --sd-req 0xB6 --application app.hex --application-version 1 app_dfu_package.zip
```

（6.x 老版把前缀换成 `nrfutil` 即可，其余参数相同。）

| 参数 | 填什么 | 填错的典型表现 |
| --- | --- | --- |
| `--hw-version` | 硬件版本号，nRF52 系列写 `52`（须与设备引导程序里的设定一致） | 手机端发起后设备立刻拒绝 |
| `--sd-req` | 设备**当前正在运行**的 SoftDevice 固件 ID（十六进制，如 `0xB6` 对应 s140 6.1.1） | 设备校验不过、拒绝升级；这是最常见的翻车点 |
| `--application` | 应用固件 `.hex`（工具会转成 `.bin` 装进包） | 文件路径错误直接报错，倒是不隐蔽 |
| `--application-version` | 应用版本号（数字），设备端可据此拒绝降级 | 版本校验被拒 |
| `--key-file` | 引导程序对应的私钥 `.pem`，生成**签名包** | 不带此参数产出未签名包，正式设备的引导程序会拒收 |
| `--debug-mode` | 跳过版本校验 | 只用于调试，正式发版别带 |

`--sd-req` 的具体取值表在工具的帮助输出里（`pkg generate --help` 会列出一批常见 SoftDevice 与 ID 的对应关系），以你设备实际运行的 SoftDevice 为准。网上示例里常见的 `0x00` 表示不校验，只适合实验。

## 四、一个包里能装哪些东西

工具帮助里明确列出了支持的组合：

- 支持单刷：引导程序（BL）、SoftDevice（SD，限同主版本）、应用（APP）
- 支持合并：BL+SD、SD+APP、BL+SD+APP
- **不支持 BL+APP**：要升这两样就打两个包、分两次升

组合填错时 `pkg generate` 会直接拒绝生成，照提示拆包即可。

## 五、老设备要打 Legacy 格式的包

DFU 包格式在 nRF5 SDK v12 换过代：

| 设备固件基于 | 包格式 | 用哪代工具 |
| --- | --- | --- |
| nRF5 SDK v12.0.0 之后 | Modern（带签名校验） | nrfutil 1.5.0 之后 / 新版 `nrf5sdk-tools` |
| nRF5 SDK v11.0.0 及更早 | Legacy（无安全校验） | 老版 nrfutil 0.5.x |

给老设备用新工具打包，设备会不认包；反过来也一样。分不清设备用的哪套，先问固件方，或直接看项目里引用的 SDK 版本。

## 六、打完先自检再传手机

- 用 `pkg display`（新版同样在 `nrf5sdk-tools` 下）查看包内容：固件类型、版本号、`sd-req` 值，逐项和设备现状核对。
- 确认包是给「这台设备」的：型号、SoftDevice、签名密钥三项对上，再拷到手机。
- 拷到手机后怎么传给设备，走 [DFU固件无线升级教程.md](DFU固件无线升级教程.md) 的升级步骤；传包失败、设备起不来的排查也在那篇。

打包参数核了三遍还是被设备拒绝，多数是 `sd-req` 或密钥不匹配。先翻 [常见问题与使用技巧.md](常见问题与使用技巧.md) 排除手机侧的因素，剩下的要直接找设备固件的提供方核对引导程序设定。
