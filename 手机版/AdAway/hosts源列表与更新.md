# AdAway hosts 源怎么加、怎么更新

> AdAway 拦什么、放什么，由 hosts 源决定。本篇讲默认源是什么、怎么加自己的源、多久更新一次，以及国内网络下会用到的重定向规则选项。相关文档：[白名单与误拦截处理.md](白名单与误拦截处理.md) · [广告没拦住怎么办.md](广告没拦住怎么办.md) · [无Root模式设置.md](无Root模式设置.md)

---

> [!IMPORTANT]
> **AdAway 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/0bef5a7b382e](https://pan.quark.cn/s/0bef5a7b382e)

---

## 一、默认自带的那个源

装好 AdAway 后，规则列表里默认就有一条官方维护的 hosts 源（AdAway default blocklist），内容以移动端广告域名和统计/分析域名为主，文件本体可以在 `https://adaway.org/hosts.txt` 直接查看。日常拦广告它基本够用，不建议一上来就堆一堆源 —— 源越多，误拦截的概率也越高。

## 二、添加自定义 hosts 源

入口在侧栏的「Hosts sources」（hosts 源）：

1. 点添加，把 hosts 文件的 URL 贴进去 —— 必须是以 hosts 格式提供的直链，普通网页链接不行。
2. 保存后回主界面执行一次「下载并应用」，新源的规则才会合并进去。
3. 不想要的源在列表里删掉，再应用一次，它的规则就移除了。

想多加几个源的话，官方 Wiki 维护了一份 [Hosts Sources 列表](https://github.com/AdAway/AdAway/wiki/HostsSources)，持续在更新。挑两三个口碑稳的加就够了，宁少勿多。单条域名的放行或强制拦截不用动源文件，直接用允许/阻止列表，见 [白名单与误拦截处理.md](白名单与误拦截处理.md)。

## 三、多久更新一次

- 广告域名天天在变，规则不会自己永远新鲜：建议每隔一两周在主界面手动执行一次「下载并应用」。
- 应用本身的升级走 F-Droid 渠道（见 [下载与安装教程.md](下载与安装教程.md)），别和规则更新混为一谈 —— 规则是应用内更新的，应用是 F-Droid 里升级的。
- 更新解决的是「规则过期」；广告和内容走同一个域名的那类（YouTube、信息流社交应用），更新多少次都拦不到，原因见 [YouTube这类广告为什么拦不住.md](YouTube这类广告为什么拦不住.md)。

## 四、国内网络的重定向规则选项

设置里有一项「Allow redirection rules from Hosts Sources」（允许来自 hosts 源的重定向规则），默认关闭。它管的是：有些源里除了拦截条目，还带「域名 → IP」的重定向条目，用来把被污染解析顶掉的域名（Google、Facebook 这类）指回正确地址。

- 只想拦广告：保持默认关闭。
- 需要这类重定向：先打开该选项，再把对应的重定向源加进 hosts 源列表（官方 Wiki 的 Hosts Sources 页里有专门的 Redirection Lists 一节）。
