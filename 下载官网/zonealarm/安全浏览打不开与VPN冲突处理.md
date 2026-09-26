# Safe Browsing 打不开与 VPN 冲突处理

> 本文解决一个问题：ZoneAlarm 的 Safe Browsing（安全浏览）开不起来、开了自己关，或和手机上其它 VPN 打架。
> **相关文档**：[首次设置与功能开启.md](首次设置与功能开启.md) · [试用订阅与常见问题.md](试用订阅与常见问题.md)

---

> [!IMPORTANT]
> **zonealarm 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/0ff123e21ea8](https://pan.quark.cn/s/0ff123e21ea8)

---

## 一、为什么它需要「VPN 权限」

Safe Browsing 是安卓版才有的功能。它不改变你的位置、也不是翻墙工具——它借用安卓系统的 VPN 通道，只检查你浏览器进出的流量，从而对所有浏览器生效的恶意站点做拦截。开成功后：功能项变绿、状态栏出现钥匙图标。

**安卓系统同一时刻只允许一个 VPN 连接**。所以只要你手机上还有别的 VPN 在跑（翻墙的、公司内网的、别的安全软件的），Safe Browsing 就会自己让位，并在首页给你报一条 Risk。

## 二、典型症状

- Safe Browsing 那一项怎么点都回不到绿色，或绿一会儿又变灰；
- 首页反复提示 Safe Browsing 有风险（Risk Detected）；
- 明明没手动关，钥匙图标自己消失了。

## 三、二选一：留 Safe Browsing，还是留你的 VPN

这是取舍，不是故障——两者只能活一个，选哪边取决于你当下更需要谁。

**方案 A：要 Safe Browsing，停掉其它 VPN**

1. 打开 ZoneAlarm → 进 **My Network（我的网络）**；
2. Safe Browsing 显示未启用时，点 **Enable VPN Permission**；
3. 在系统弹出的连接请求里点 **OK**，功能变绿即恢复；
4. 期间别再启动其它 VPN 应用。

**方案 B：要自己的 VPN，彻底关掉 Safe Browsing**

1. ZoneAlarm → **My Network** → 点 **Disable VPN Permissions**；
2. 在弹出的提示里点 **Disable**，跳转到系统的 VPN 配置页（没跳转就按提示手动进「设置 → VPN」）；
3. 找到 ZoneAlarm 的 VPN 配置，选择 **编辑 → 删除/忘记该 VPN**；
4. 回到应用，Risk 提示消失，首页恢复绿色。

## 四、还是不行时的三件小事

1. **确认只有一个安全类应用在抢 VPN**：有些「加速器」「清理大师」也会挂 VPN 配置，把它们的 VPN 关掉再试；
2. **别用 Mute 掩盖提示**：风险提示旁的 Mute 只是静音，不解决任何问题，关了提示照样没防护；
3. **重开一次应用**：改完系统 VPN 配置后回应用刷新状态，个别机型需要把应用切到后台再打开才会更新。

如果你压根还没装上，先回 [下载与安装教程.md](下载与安装教程.md)；试用期相关的问题见 [试用订阅与常见问题.md](试用订阅与常见问题.md)。
