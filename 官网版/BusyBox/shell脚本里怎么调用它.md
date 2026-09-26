# shell 脚本里怎么调用 BusyBox

> 写自动化脚本时怎么指明用哪一份命令、怎么避免在别的机器上跑挂。
> **相关文档**：[常用命令速查.md](常用命令速查.md) · [装好后命令不生效的排查.md](装好后命令不生效的排查.md) · [下载与安装教程.md](下载与安装教程.md)

---

> [!IMPORTANT]
> **BusyBox 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/eb2f313d9c4a](https://pan.quark.cn/s/eb2f313d9c4a)

---

## 一、脚本里先指明用哪一份

把路径存进变量，之后一律通过它调用：

```sh
#!/system/bin/sh
BB=/system/xbin/busybox   # 换成你安装时选的路径

"$BB" grep "error" /data/log.txt
"$BB" chmod 755 /data/local/tmp/job.sh
```

好处是脚本换到另一台机器上，只要改开头那一行就能跑：不用赌 PATH 里恰好先找到哪份 `grep`，也不用管那台机器装没装。同理，想让整个脚本都用它的 shell 跑，就整段交给它执行：

```sh
/system/xbin/busybox sh /sdcard/job.sh
```

只想临时跑一次命令时，用 `busybox 命令` 的敲法就够了，不必依赖装没装链接。

## 二、几个用得上的场景

```sh
#!/system/bin/sh
BB=/system/xbin/busybox

# 批量给目录下的脚本加执行权限
for f in /data/local/tmp/*.sh; do
  "$BB" chmod 755 "$f"
done

# 核对下载的文件是否完整
"$BB" md5sum /sdcard/BusyBox_v71.apk

# 只把今天日志里带关键字的行导出一份
"$BB" grep "Exception" /data/log.txt > /sdcard/errors.txt
```

这类脚本通常要在 root 下跑（改 `/data` 下的文件、碰系统目录），所以在开头先 `su` 或由 root 终端发起；只动 `/sdcard` 的部分不加 root 也能执行。

## 三、为什么不让脚本直接用系统自带命令

系统自带的命令集是另一套实现，选项覆盖和 BusyBox 不完全一致：同一条 `grep`，教程里的某个参数可能这边不认，脚本就静默少过滤了一批行 —— 这种错不报异常，最难查。脚本里统一走 `$BB` 前缀，等于把运行环境钉死，行为在你手上可控。

反过来，BusyBox 的实现也是精简版，选项比 GNU 少。遇到 `unrecognized option` 就查它自己的帮助：

```sh
busybox grep --help
```

按它支持的写法改脚本，别按电脑上 GNU 工具的习惯硬套。

## 四、让整个 shell 里的命令都走它：standalone 模式

上面的 `$BB` 前缀要一条条加，还管不到脚本里直接写的 `ls`、`rm`。有一种开关能让 shell 里**每条命令都强制走这份二进制**，不管 PATH 里排的是谁 —— 这正是 Magisk 跑自己脚本时的做法（官方文档写明：Magisk 的所有启动脚本与模块安装脚本都在这种模式下执行）。两种打开方式：

```sh
# 方式一：环境变量（推荐，会传给脚本里新起的子 shell）
ASH_STANDALONE=1 /data/adb/magisk/busybox sh /sdcard/job.sh

# 方式二：命令行开关
/data/adb/magisk/busybox sh -o standalone /sdcard/job.sh
```

上面用的是 Magisk 内置那份的路径；**你装的这份支不支持这个开关，先验一下再用**：

```sh
busybox sh --help
```

输出里有 `standalone` 相关选项才可用。用上之后，脚本里的 `ls` 调的就是这份的 `ls`，不再受 PATH 影响；想让某一条命令绕开它，就写绝对路径（如 `/system/bin/ls`）。

## 五、定时任务这类后台用法

它自带 `crond`、`crontab` 这组命令，但**能不能真跑起来取决于你给不给 root 和后台权限**，不同 root 方案差别不小。想折腾定时任务，先敲：

```sh
busybox crond --help
```

看清用法与日志位置再配，配完记得回头验证任务真的执行了（看有没有产出文件），别只看配置写没写。若你只是偶尔手动跑脚本，这一节可以完全跳过。
