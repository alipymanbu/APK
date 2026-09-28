# Pixiv Viewer RefreshToken 获取步骤

> 本篇讲为什么建议用 RefreshToken 登录 Pixiv Viewer，以及拿到它的几种途径——最省事的一种，你装好的这个应用自己就能完成。
> **相关文档**：[登录与账号设置.md](登录与账号设置.md) · [常见问题与加载失败排查.md](常见问题与加载失败排查.md)

---

> [!IMPORTANT]
> **Pixiv Viewer 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f98ba6b68ced](https://pan.quark.cn/s/f98ba6b68ced)

---

## 一、为什么要走 RefreshToken

官方 FAQ 的口径很明确：Cookie / SessionID 方式容易失效出错，RefreshToken 是最稳的登录方式；而且 RefreshToken 的有效期较长，登录成功一次、妥善保存，之后在支持 Token 登录的应用里都能直接用，不必反复登。

## 二、前提

- 有一个自己的 Pixiv 账号（没有就先去官网注册一个）。
- 操作时你能正常打开 Pixiv 的官方登录页（这一点取决于你平时的上网条件，本篇不展开）。

## 三、方式一：直接在 Pixiv Viewer 里登录（推荐）

官方教程（`https://www.nanoka.top/posts/e78ef86/`）里给本应用准备的流程：

1. 打开 Pixiv Viewer，进「设置 → 登录」。
2. 选择 `App API (OAuth)` 登录，应用会拉起 Pixiv 官方登录页面。
3. 在官方页面上登录你的账号，成功后回到应用。
4. 事后在「设置 → 其他设置」里可以导出 RefreshToken，妥善保存，供其他支持 Token 登录的工具复用。

整个流程不装任何额外东西，普通用户到这一步就够用了。

## 四、方式二：网页端登录后导出

同样出自官方教程，适合想在电脑浏览器里操作的情况：

1. 浏览器安装 Redirector 扩展并导入官方规则文件，再装 Tampermonkey 并安装官方登录辅助脚本（各下载地址教程页里都给了）。
2. 打开 `https://pixiv.pictures/account/login`，选择 `App API (OAuth)` 登录，会跳到 Pixiv 官方登录页。
3. 登录成功后在站点设置页导出 RefreshToken。

安卓浏览器上操作建议用 Firefox 或 Edge Canary。每一步的具体安装地址以教程页（`https://www.nanoka.top/posts/e78ef86/`）当时显示为准。

## 五、脚本方式（进阶）

- **Node.js**：安装 pxder 后用 `pxder --login` 登录、`pxder --export-token` 导出。
- **Python**：gppt 等现成脚本，装好依赖按说明执行。

这类方式适合要在多处复用 Token、或想写成自动化的人；只是想登录 Pixiv Viewer 的话，用上面两种就够了，不必折腾脚本。

## 六、拿到 Token 之后与保管

- 粘贴进 Pixiv Viewer 的登录设置即可，登录方式的选择对照见[登录与账号设置.md](登录与账号设置.md)。
- RefreshToken 等同于账号的钥匙：不要发给任何人、不要贴进公开场合；怀疑泄露就去 Pixiv 官网改密码并重新获取。
- 登录过程或登录后报错，对照[常见问题与加载失败排查.md](常见问题与加载失败排查.md)第六节处理。
