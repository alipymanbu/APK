# WebToApp 油猴脚本和 Chrome 扩展怎么导入

> 本篇讲清导入 `.user.js` 油猴脚本与 Chrome MV3 扩展的格式要求、支持哪些接口、体积上限，以及导入后不生效时的排查顺序。
> **相关文档**：[扩展模块与脚本注入怎么用.md](扩展模块与脚本注入怎么用.md) · [浏览器内核选WebView还是GeckoView.md](浏览器内核选WebView还是GeckoView.md) · [预览正常但导出后失效怎么办.md](预览正常但导出后失效怎么办.md)

---

> [!IMPORTANT]
> **WebToApp 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/98da99a44c60](https://pan.quark.cn/s/98da99a44c60)

---

比临时写一段注入脚本更省事的做法，是把现成的搬进来：你在浏览器上已经调好的脚本资产，有相当一部分可以直接复用。WebToApp 同时支持 Tampermonkey / Greasemonkey 风格的 `.user.js`（带 `GM_*` 接口）和 Chrome MV3 格式的扩展，导入后与其他扩展一样被解析、存储和注入。

## 一、油猴脚本必须有元数据块

脚本开头必须有 `// ==UserScript==` 到 `// ==/UserScript==` 这一段。**没有它，文件会按普通 JS 导入**，不会被当成油猴脚本处理 —— 这是「导入成功但什么也没发生」最常见的原因。

```javascript
// ==UserScript==
// @name         Example Script
// @namespace    https://example.com/
// @version      1.0.0
// @description  Does a thing
// @author       You
// @match        *://example.com/*
// @grant        GM_getValue
// @grant        GM_setValue
// @grant        GM_xmlhttpRequest
// @run-at       document-idle
// @noframes
// ==/UserScript==

console.log('Hello from a userscript')
```

**被识别的 `@` 标签**：`@name`、`@description` / `@desc`、`@version`、`@author`、`@namespace`、`@match`、`@include`、`@exclude` / `@exclude-match`、`@run-at`、`@grant`、`@require`、`@resource`、`@noframes`、`@icon` / `@iconURL` / `@icon64` / `@icon64URL`、`@homepage` / `@homepageURL` / `@website`。

**匹配规则上有一处需要注意**：`@match` 按 Chrome 的 glob 写法解析，`<all_urls>` 要写成 `*`；`@include` 写成 `/正则/` 形式时按正则处理。

**`@run-at` 的三档映射**：

| 脚本里写 | 实际时机 |
| --- | --- |
| `document-start` | 页面开始加载时 |
| `document-end` / `document-body` | 页面结构解析完毕后 |
| `document-idle`（默认） | 页面空闲时 |

## 二、能用的 `GM_*` 接口

同时提供经典 `GM_*` 函数和 Tampermonkey 4.x 风格的 Promise 版 `GM.*`（两者一一对应）。

| 接口 | 能做什么 | 注意 |
| --- | --- | --- |
| `GM_getValue` / `GM_setValue` / `GM_deleteValue` / `GM_listValues` | 读写脚本自己的配置 | 按脚本命名空间隔离，脚本之间互不干扰 |
| `GM_xmlhttpRequest` | 发起网络请求 | 支持 `method`、`url`、`headers`、`data`、`responseType` 与 `onload` / `onerror` / `ontimeout` / `onprogress` |
| `GM_addStyle` | 注入样式表 | |
| `GM_setClipboard` | 写入剪贴板 | |
| `GM_openInTab` | 在新标签页打开网址 | |
| `GM_log` | 输出日志 | |
| `GM_notification` | **只记录日志，不弹系统通知** | 依赖系统通知的脚本会静默失败 |
| `GM_getResourceText` / `GM_getResourceURL` | 读取 `@resource` 内容 | |
| `GM_registerMenuCommand` / `GM_unregisterMenuCommand` | 把入口挂到浮窗菜单 | |
| `GM_info` | 读取脚本元信息 | `scriptHandler` 为 `WebToApp` |
| `unsafeWindow` | 访问页面全局对象 | 无沙箱，等于页面自己的 `window` |

一个容易踩的点：**`@grant` 声明会被解析，但接口其实无条件可用** —— 不写 `@grant` 也能调 `GM_*`。不过为了脚本在真正的 Tampermonkey / Greasemonkey 上也能跑，仍然建议照常声明。

## 三、`@require` 与 `@resource` 的体积上限

这两者引用的外部文件会被下载并缓存，然后在脚本主体之前注入。注入顺序是：窗口管理引导 → 兼容层 → 各 `@require` → 你的脚本。

| 限制 | 上限 |
| --- | --- |
| 单个 `@require` | 5 MB |
| 整个扩展包 | 50 MB |

**超过上限会直接失败**，这是「脚本本身没错、就是装不上」的常见原因。遇到这种情况，把依赖改成运行时按需请求，或者精简掉用不到的部分。

## 四、内核与扩展的兼容性

**GeckoView 内核不支持用户脚本与 Chrome MV3 扩展** —— 依赖这些能力的应用必须选择系统 WebView 内核，判断方法见 [浏览器内核选WebView还是GeckoView.md](浏览器内核选WebView还是GeckoView.md)。

## 五、导入后不生效怎么查

按这个顺序，每步都能缩小范围：

1. **确认元数据块存在**：没有 `// ==UserScript==` 块，文件是按普通 JS 导入的；
2. **确认 `@match` 真的能匹配到你的网址**：写成 `*://example.com/*` 而实际访问的是别的域名或子域，脚本不会执行；
3. **确认 `@run-at` 时机合适**：默认的 `idle` 时机拿不到页面早期状态时，改成 `document-start` 再试；
4. **确认内核是系统 WebView**：GeckoView 下这类脚本一律不生效；
5. **确认脚本被带进了产物**：预览里能用、装上不能用，按 [预览正常但导出后失效怎么办.md](预览正常但导出后失效怎么办.md) 核对配置保存与素材打包。

想改用自己写的一段脚本或现成模块，见 [扩展模块与脚本注入怎么用.md](扩展模块与脚本注入怎么用.md)。
