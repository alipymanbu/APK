# 免 root 改设置常用 adb 命令合集

> 在 ShizukuRunner 的格子里直接能跑的命令，按用途分组。填写时去掉 `adb shell` 前缀，只填命令本体。

## 使用前提

Shizuku 已激活、ShizukuRunner 已在「已授权应用」里打开开关（流程见 [无线调试激活与授权步骤.md](无线调试激活与授权步骤.md)）。没激活时 v17 起 Runner 会直接禁止执行，点了不会有任何效果。

**改任何值之前先跑一次 get 类命令记下原值**；改出问题靠原值恢复。执行成功通常没有输出（静默完成），出现红色提示才是没跑成。

## 屏幕与显示

```sh
wm density
wm density 420
wm density reset
```

第一条查当前密度，第二条按需改值，第三条恢复默认。分辨率同理：`wm size` 查、`wm size 1080x2400` 改、`wm size reset` 还原。

## 动画速度（界面更跟手）

```sh
settings put global window_animation_scale 0.5
settings put global transition_animation_scale 0.5
settings put global animator_duration_scale 0.5
```

0.5 是半速、1 是系统默认；恢复时把三个值改回 1 再跑一遍。这组是 AOSP 通用键，绝大多数品牌都认。

## 应用管理

```sh
pm list packages -3
pm uninstall --user 0 包名
pm install-existing 包名
```

第一条列出第三方应用；卸载系统应用优先用 `--user 0`（只对当前用户隐藏，还能用 `pm install-existing` 找回）。桌面、系统界面这类包别碰，卸了可能直接黑屏。

## 亮度 / 休眠 / 常亮

```sh
settings get system screen_brightness
settings put system screen_brightness 150
settings put system screen_off_timeout 600000
svc power stayon true
```

亮度取值 0~255，先查再改；锁屏时长单位毫秒（600000 是 10 分钟）；常亮只在充电时生效，关闭把 true 换成 false。

## 两个执行期的注意事项

- 输出超过 1000 字符会被 Runner 主动掐断、返回值 141——这是防卡死保护。想看长输出就把结果重定向到文件；
- `logcat` 这类永远有输出又停不下来的命令别在手机上裸跑，Shizuku 通道收不到终止控制码，进程会一直占着。

命令跑通后想进一步了解工具本身（格子墙玩法、备份导入导出），见 [免root跑adb命令的工具.md](免root跑adb命令的工具.md)；安装文件资源与真机截图都在 [ShizukuRunner 介绍站](https://shizukurunner.qhah.skin/)。
