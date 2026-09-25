# TagInfo 支持哪些NFC标签和交通卡

> 这篇列 TagInfo 能识别的标签芯片、NFC Forum 标签类型，以及交通卡查询的覆盖范围。
> **相关文档**：[扫描结果怎么看.md](扫描结果怎么看.md) · [原厂真伪校验怎么用.md](原厂真伪校验怎么用.md) · [读不到卡怎么办.md](读不到卡怎么办.md)

---

> [!IMPORTANT]
> **taginfo 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/15e32b7370fe](https://pan.quark.cn/s/15e32b7370fe)

---

## 一、先说结论

TagInfo 的兼容面覆盖了市面上绝大多数 NFC 标签：NFC Forum 四类标签标准、NXP 的 NTAG / MIFARE / ICODE 全系，以及不少别家芯片。交通卡这类带应用的复合卡，它能识别卡上的应用；余额能不能查，取决于该卡的系统方有没有开放访问方式——官方的立场是「拿到系统方提供的访问信息才会开通对应卡种」。所以**别把「支持某交通卡」理解成「一定能查余额」**。

## 二、NFC Forum 标签类型

以下四类全部支持完整的内容浏览与分析：

| 类型 | 典型用途 |
| --- | --- |
| Type 2 | 最常见的贴纸标签，NTAG21x 属于这一类 |
| Type 4 | 容量较大、支持多种应用的标签 |
| Type 5 | 基于 ISO/IEC 15693 的远距离标签，ICODE 系列属于这一类 |
| Type 1 | 老一代标签，市面上已少见 |

## 三、支持的芯片系列（按产品家族）

官方列出的支持范围（节选常见型号，完整清单以 NXP 官网产品页为准）：

- **NTAG 系列**：NTAG213 / 215 / 216、NTAG223 / 224 DNA、NTAG424 DNA（及 TagTamper）、NTAG210 / 210u、NTAG I2C Plus
- **MIFARE Ultralight 系列**：Ultralight、Ultralight C、Ultralight EV1、Ultralight AES
- **MIFARE Classic 系列**：Classic EV1 1K / 4K 等
- **MIFARE DESFire 系列**：DESFire Light、EV2、EV3（含各容量版本）
- **MIFARE Plus 系列**：Plus EV2
- **ICODE 系列**：ICODE SLIX、SLIX-L、SLIX2、ICODE DNA、ICODE 3（含 TT）
- **其他**：MIFARE DUOX 等较新型号（新版本陆续加入识别支持）

原厂真伪校验（Originality Check）对其中哪些家族生效，单独放在 [原厂真伪校验怎么用.md](原厂真伪校验怎么用.md)。

## 四、交通卡与支付卡：能识别 ≠ 能读内容

### 1. 余额查询（Value Checker）

官方描述里点名支持余额查询的交通系统包括（**名单随版本变化，以应用内实际结果为准**）：

- Suica（日本）、Octopus 八达通（中国香港）、Oyster（英国伦敦）
- ORCA、CLIPPER（美国）、OV-chipkaart（荷兰）、Myki（澳大利亚）
- Kiev Metro、Moscow Metro（乌克兰/俄罗斯）、Nol Silver/Gold（迪拜）、T:kort（挪威）等

这些卡能查余额的前提是卡内该应用允许读取。国内多数城市的交通联合卡**不在此名单内**，扫出来通常只能看到卡上的应用名，看不到金额——这不是手机或应用的问题。

### 2. 支付卡与证件类

银行卡（Visa payWave、MasterCard PayPass、American Express ExpressPay、Discover Zip）与电子身份证这类应用，TagInfo **能识别出应用是什么**，但因卡片安全机制，**读不到实际数据**。官方对此有明确说明，属于预期行为。

### 3. 判断一张卡属于哪种情况

扫完对照 [扫描结果怎么看.md](扫描结果怎么看.md) 的结果页：

- 有芯片型号、有 NDEF 内容 → 完全可读；
- 只有应用名、内容区是空的或提示受保护 → 卡片加密，正常；
- 连芯片型号都出不来 → 多半是没贴对位置或卡本身不是 13.56 MHz，按 [读不到卡怎么办.md](读不到卡怎么办.md) 排查。
