# 动画JSON格式与素材哪里找

> 讲 Lottie 动画文件是什么、三种扩展名各是什么、动画素材从哪来，以及它和 LottieFiles 那款手机 App 的区别。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [本地动画和网络动画怎么加载播放.md](本地动画和网络动画怎么加载播放.md) · [动画打不开或加载失败怎么排查.md](动画打不开或加载失败怎么排查.md)

---

> [!IMPORTANT]
> **Lottie 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/cd44c6634131](https://pan.quark.cn/s/cd44c6634131)

---

## 一、一个 JSON 描述一整段动画

Lottie 的做法是：设计师在 Adobe After Effects 里把动画做完，用 **Bodymovin** 插件导出成一个 JSON 文件，这个文件里记录了每一层、每一帧的形状与位移；播放端（手机、网页）读这个 JSON，**在本机实时画出来**，而不是播放一段视频。

所以你下载到的「动画」常常就是一个几十 KB 的文本文件。官方 README 给过一组体积对照，用来说明为什么要这么做：同样一段动画，GIF 的体积是 JSON 的两倍多，PNG 序列帧能到 JSON 的 30~50 倍，而且 GIF 与 PNG 序列的尺寸是固定的，放到高分屏上没法放大。这组对照是官方给的口径，实际选哪种格式，你自己按项目需求判断。

顺带一提名字来源：Lottie 取自德国剪影动画先驱 **Lotte Reiniger**，她 1926 年的《阿基米德王子历险记》是现存最早的长篇动画电影。

## 二、三种扩展名，别拿错

| 扩展名 | 是什么 | 什么时候用 |
| --- | --- | --- |
| `.json` | 标准 Lottie 动画，Bodymovin 直接导出 | 动画只用矢量形状时，这一个文件就够 |
| `.zip` | json + 图片素材打包在一起 | 动画里嵌了位图；**少了图片就会白一块**（处理办法见 [动画打不开或加载失败怎么排查.md](动画打不开或加载失败怎么排查.md) 第二节） |
| `.lottie` | dotLottie，LottieFiles 推出的另一种打包格式，一个文件里可以装多个动画与配置 | 平台下载时会遇到；本地播放器支持到哪种，以你装的版本为准 |

判断一个文件到底是不是 Lottie：用文本编辑器打开，开头是 `{`、里面有 `"v"` `"fr"` `"layers"` 这类键，就是 json 动画；打不开的压缩包则是 zip 或 lottie。

## 三、动画素材哪里找

- **LottieFiles**（[www.lottiefiles.com](https://www.lottiefiles.com/)）：独立于 Airbnb 的第三方平台，设计师在上面上传、预览、下载动画，也有在线 JSON 编辑器可以改颜色与时长。**注意它是另一家公司做的**，与你手机上装的这款官方示例应用没有从属关系。
- **官方文档** [lottie.airbnb.tech](https://lottie.airbnb.tech/)：各平台接入说明、支持哪些 After Effects 特性、性能注意点都在这里。
- **开源仓库** [github.com/airbnb/lottie-android](https://github.com/airbnb/lottie-android)：安卓端实现与示例动画源文件，示例应用自带的那批动画就来自这里。

下载动画时优先挑**只用矢量**的文件——带图片的不仅体积大，还会遇到上面说的打包问题。

## 四、别装错：两款手机 App 的区别

| | 本文这一款 | LottieFiles 手机版 |
| --- | --- | --- |
| 怎么认 | 包名 `com.airbnb.lottie` | 应用名与开发者署名都是 LottieFiles |
| 出品 | Airbnb（Lottie 的开源方） | LottieFiles 平台 |
| 定位 | 官方示例播放器，验证本地/网络动画 | 平台客户端，扫码预览平台上的动画、做模板编辑 |

两者都能在手机上看到 Lottie 动画，但来源与玩法不同。你要的是「验自己手里的动画文件」，装本文这款；要的是「逛素材、改模板」，用 LottieFiles 的。安装包与版本信息见 [下载与安装教程.md](下载与安装教程.md)。

## 五、要接进自己应用的人看这里

安卓项目里加依赖即可（Gradle 是官方唯一支持的构建方式）：

```groovy
dependencies {
    implementation "com.airbnb.android:lottie:$lottieVersion"
}
```

版本号去 [Maven 中央仓库的 lottie 目录](https://search.maven.org/artifact/com.airbnb.android/lottie) 或 GitHub 的 releases 页查，**以官方页面当时显示为准**。三条实操建议：

1. **库版本别太旧**：新导出的 json 可能旧库读不了，报错参考 [动画打不开或加载失败怎么排查.md](动画打不开或加载失败怎么排查.md) 第五节。
2. **动画打进包里还是走网络**：放进 `res/raw` 或 `assets` 随应用一起发布，断网也能播；走网络加载要自己处理失败兜底。
3. **上线前在真机上验一遍**：把 json 用 [本地动画和网络动画怎么加载播放.md](本地动画和网络动画怎么加载播放.md) 里说的办法在手机上过一遍，比在编辑器里看着差不多可靠。
