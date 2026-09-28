# Tricky Store keybox.xml 配置指南

> `keybox.xml` 是 Tricky Store 的可选密钥文件，官方定位是「想要超过 DEVICE 级别的完整性结果时才需要」。本篇讲它放哪、格式长什么样、吊销了怎么办。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [target.txt配置详解.md](target.txt配置详解.md) · [常见问题与排查.md](常见问题与排查.md)

---

> [!IMPORTANT]
> **Tricky Store 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a8f462b3607c](https://pan.quark.cn/s/a8f462b3607c)

---

## 一、先想清楚要不要放

不放 `keybox.xml`，模块照样工作——官方文档把它列为**可选**步骤，并写明了它的定位：要拿到超过 DEVICE 级别的完整性结果时才用得上。所以如果你只是做基础配置，先跳过本篇，把 [target.txt配置详解.md](target.txt配置详解.md) 配好即可；等确实需要更高一级的结果时再回来。

## 二、放哪、叫什么

路径和文件名都是固定的：

```text
/data/adb/tricky_store/keybox.xml
```

用带 root 权限的文件管理器把文件放进这个目录即可。文件名必须一字不差，放成 `keybox.txt` 或者放进子目录都不行。和 `target.txt` 一样，**保存后立即生效**，不用重启。

## 三、文件格式

`keybox.xml` 有固定的 XML 结构，包含私钥（PEM 格式）和证书链，算法支持 `ecdsa` 与 `rsa`。官方文档给出的骨架如下，实际使用时把 `...` 的位置换成真实内容：

```xml
<?xml version="1.0"?>
<AndroidAttestation>
    <NumberOfKeyboxes>1</NumberOfKeyboxes>
    <Keybox DeviceID="...">
        <Key algorithm="ecdsa|rsa">
            <PrivateKey format="pem">
-----BEGIN EC PRIVATE KEY-----
...
-----END EC PRIVATE KEY-----
            </PrivateKey>
            <CertificateChain>
                <NumberOfCertificates>...</NumberOfCertificates>
                <Certificate format="pem">
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
                </Certificate>
                ... more certificates
            </CertificateChain>
        </Key>...
    </Keybox>
</AndroidAttestation>
```

几个容易出错的点：

- `NumberOfCertificates` 的数字要和下面实际写的证书张数一致；
- 私钥与证书都是 PEM 文本，直接贴进 XML，不是 base64 单行串；
- 文件是 XML，编码存成 UTF-8，别带 BOM。

## 四、吊销了怎么办

官方对 keybox 的要求是「未被吊销的（unrevoked）」。社区教程普遍反映：keybox 会被不定期吊销，传播越广的越容易被吊销；吊销后原本能拿到的结果会掉回去。

处理办法不复杂：

1. 表现可疑时（之前好好的、某天突然不行了），先怀疑 keybox 失效，而不是急着改配置；
2. 换一份新的 keybox.xml，覆盖 `/data/adb/tricky_store/keybox.xml` 同名同路径；
3. 保存即生效，重新验证即可。

换句话说，keybox 不是「配一次管终身」的东西，把它当成会过期的耗材来看。

## 五、两条提醒

1. **这是私钥材料**。`keybox.xml` 里是货真价实的私钥，别把它发到公开场合（截图、传网盘公开分享、贴论坛）。不用了就删掉，见 [下载与安装教程.md](下载与安装教程.md) 卸载一节的说明。
2. **来源自担**。官方只定义了格式，不提供 keybox 文件；网上流传的文件内容无法逐一验证，用谁的、信到什么程度，你自己判断。
