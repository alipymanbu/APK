# Reex 挂载 Anime4K 着色器提升画质

> 想让 720p/1080p 的动漫与老片在手机上更锐利：把开源的 Anime4K 着色器挂进 Reex 的完整步骤。
> **相关文档**：[播放卡顿与常见问题排查.md](播放卡顿与常见问题排查.md) · [本地视频播放与倍速手势设置.md](本地视频播放与倍速手势设置.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **Reex 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/f6e8601148a0](https://pan.quark.cn/s/f6e8601148a0)

---

## 一、这是什么、适合谁

Anime4K 是一套开源的实时动漫放大/降噪着色器（项目地址 [github.com/bloc97/Anime4K](https://github.com/bloc97/Anime4K)，MIT 许可），通过 GLSL 文件在播放时实时锐化画面、柔化锯齿。它不是 Reex 的内置功能 —— Reex 基于 libmpv，能加载 mpv 的着色器机制，所以把 Anime4K 的 `.glsl` 文件放进配置目录、在 `mpv.conf` 里引用，就能用上。

适合看 **720p / 1080p 动漫、画质一般的老番** 的场景；你手上的片源本来就是 4K 高码率，这一步收益不大。项目自述其放大效果可把 1080p 内容提升到接近 2160p 的观感（项目页面的说法，实际因片源与机型而异）。

## 二、安装四步

1. **下载着色器文件**：到 [github.com/bloc97/Anime4K/releases](https://github.com/bloc97/Anime4K/releases) 下载 GLSL 版本的压缩包（文件名含 `GLSL`，如 `Anime4K_GLSL.zip`，以页面当时显示为准）。
2. **解压到配置目录**：把压缩包里的全部 `.glsl` 文件解压到手机上的这个目录：

   ```text
   /storage/emulated/0/Android/data/xyz.re.player.ex/files/mpv/shaders/
   ```

   没有 `shaders` 文件夹就手动新建一个。这一步通常要用文件管理器（部分机型路径较长，建议用支持长路径的管理器）。
3. **编辑 mpv.conf**：在 `mpv/` 目录下新建或编辑 `mpv.conf`，写入以下五行（社区通行的最低档配置）：

   ```text
   glsl-shaders-append="~~/shaders/Anime4K_Clamp_Highlights.glsl"
   glsl-shaders-append="~~/shaders/Anime4K_Restore_CNN_S.glsl"
   glsl-shaders-append="~~/shaders/Anime4K_Upscale_Denoise_CNN_x2_S.glsl"
   glsl-shaders-append="~~/shaders/Anime4K_AutoDownscalePre_x2.glsl"
   glsl-shaders-append="~~/shaders/Anime4K_Deblur_DoG.glsl"
   ```
4. **切换硬解模式并重启**：打开 Reex → 设置 → 解码，把硬件解码切到 `mediacodec-copy`，然后完全退出应用重进，播一集动漫看效果。

## 三、档位怎么调

上面五行里的 `_S` 后缀是**最小性能开销**的一档。对它有信心（手机芯片较强、播放不掉帧）后，可以把文件换成 `_M` 或更大档位的同名文件，放大与降噪会更明显 —— 各档位的取舍以 [Anime4K 项目文档](https://github.com/bloc97/Anime4K/blob/master/GLSL_Instructions.md) 为准（文件名与档位随项目更新变化，以页面当时显示为准）。

判断标准很直接：**播放同一段片子，掉帧/发热明显就退回 S 档或删掉 `mpv.conf` 恢复默认**。

## 四、没效果或变卡了

| 现象 | 先查什么 |
| --- | --- |
| 画面没有任何变化 | 硬件解码是否切到了 `mediacodec-copy`（社区反馈其它模式下着色器不生效）；`mpv.conf` 是否写错引号或路径；改完是否完全退出并重开了应用 |
| 播放明显变卡、掉帧 | 手机性能不够跑这些着色器 —— 删掉 `mpv.conf` 里那五行即可恢复默认，不影响其它功能 |
| `.glsl` 文件找不到 | 目录路径逐字核对，注意是 `xyz.re.player.ex` 而不是别的包名；确认文件解压在 `shaders/` 里而不是再套了一层文件夹 |

配置目录与 `mpv.conf` 的更多用法见 [播放卡顿与常见问题排查.md](播放卡顿与常见问题排查.md) 第五节；改动前先备份原文件。
