# SuperSU 完整卸载与取消 root 方法

> 本篇讲怎么把 SuperSU 和 root 一起干净地退掉：为什么不能直接卸载、应用内 Full unroot 的步骤、TWRP 刷卸载包的兜底方案，以及换其他 root 方案时的正确顺序。
> **相关文档**：[下载与安装教程.md](下载与安装教程.md) · [root权限授权与日常管理.md](root权限授权与日常管理.md) · [与Magisk的区别及怎么选.md](与Magisk的区别及怎么选.md)

---

> [!IMPORTANT]
> **SuperSU 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/ae57de1aba73](https://pan.quark.cn/s/ae57de1aba73)

---

## 一、先记住：别在系统设置里直接卸载

SuperSU 不是普通应用。root 的关键部件（su 二进制）落在系统层，桌面上的 SuperSU 只是个管理界面：

- 直接卸载 APK 等于只拆了壳，su 二进制还在，root 状态照样存在，却**没有管理器管它**；
- su 二进制在系统里**同时只能有一套**，换其他管理器时也要讲究先后顺序（见第四节）。

所以应用描述里专门写着：想删它，走专门流程，别直接卸载。

## 二、方法一：应用内 Full unroot（推荐先试）

这是最省事的路子，SuperSU 自带：

1. 打开 SuperSU，进「设置」（右上角菜单）；
2. 往下翻到**清理（Cleanup）**区域，点 **Full unroot（完整取消 root）**；
3. 弹窗确认，点「继续」；
4. 跑完后**重启手机**。

成功的标志：重启后 SuperSU 图标消失，root 类工具再要权限一律失败。

## 三、方法二：TWRP 刷卸载包（兜底）

Full unroot 失败、一直停在"处理中"，或者系统已经不太正常时，用 recovery 里的卸载包处理：

1. 准备一份 SuperSU Uninstaller 类 ZIP（社区维护的 `UPDATE-unSU-signed.zip` 即属此类）；
2. 放进手机存储，进 TWRP；
3. **Install** → 选中卸载包 → 滑动确认刷入；
4. 完成后 Wipe cache/dalvik（可选），**Reboot System**。

开机后 SuperSU 应已消失。这一步只拆 root，不动你的数据分区。

## 四、想换别的 root 方案，顺序不能错

**换回其他 Superuser 管理器**（Superuser 类应用）：

1. 先装上新管理器，在它里面执行"安装/更新 su 二进制"，让**它**接管系统里的 su；
2. 确认几个常用 root 工具的授权请求已经改由新管理器弹出；
3. 这时候再卸载 SuperSU——顺序反了会出现两个管理器抢一个 su、或者谁也管不了的中间态。

**升级到 Magisk**：Magisk 走的是另一条路（修补 boot 镜像），和 SuperSU 的 su 二进制不共存。先把上面的 Full unroot 做干净、重启确认，再按 Magisk 自己的流程从头装，两者差异见 [与Magisk的区别及怎么选.md](与Magisk的区别及怎么选.md)。

## 五、卸载后怎么确认真的干净

- 装个 root 检测类工具跑一遍，结果应为"未 root"；
- 试一次系统更新（OTA）：boot 分区被改过的机型可能仍提示异常，必要时按机型教程重刷原版 boot 或线刷；
- 之前因 root 被拒的银行、支付类应用，重启后应恢复正常。

卸载完又反悔想装回来？安装包就用开头的 [SuperSU 安装文件资源（夸克网盘）](https://pan.quark.cn/s/ae57de1aba73)，装回流程照 [下载与安装教程.md](下载与安装教程.md) 再走一遍即可。
