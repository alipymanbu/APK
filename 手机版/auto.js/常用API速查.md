# Auto.js 常用 API 速查

> 写脚本时最常翻的模块与函数对照表，都按 Auto.js 4.x 的口径整理，查细节以官方文档为准。
> **相关文档**：[脚本编写入门.md](脚本编写入门.md) · [打包与VSCode开发.md](打包与VSCode开发.md) · [无障碍服务开启教程.md](无障碍服务开启教程.md)

---

> [!IMPORTANT]
> **Auto.js 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/a6758b43ec4b](https://pan.quark.cn/s/a6758b43ec4b)

---

Auto.js 的 API 按模块划分，脚本里可以直接调用，不用 import。下表是使用频率最高的一批，每一格都给了最小可用的写法。

## 一、启动应用

| 函数 | 用法 | 说明 |
| --- | --- | --- |
| `launchApp(name)` | `launchApp("微信");` | 按应用名启动，名字对不上返回 false |
| `launch(pkg)` | `launch("com.tencent.mm");` | 按包名启动，更可靠 |
| `app.getPackageName(name)` | `app.getPackageName("QQ");` | 应用名反查包名 |
| `app.openUrl(url)` | `app.openUrl("https://github.com/hyb1996/Auto.js");` | 用浏览器打开网页 |

## 二、找控件与操作控件

```javascript
// 按属性定位，常见选择器：text / desc / id / className
let btn = id("submit").findOne(5000);   // 最多等 5 秒
if (btn) btn.click();

// 链式条件：文字匹配正则
textMatches(/继续|确定/).findOne().click();

// 找不到时直接返回 null，避免 findOne 永远等待
let maybe = text("下一步").findOnce();   // 只找一次，不等待
```

要点：`findOne()` 不传参数会一直等到找到为止，脚本「卡住不动」多半是这里；调试期一律传超时毫秒数。

## 三、坐标与手势（不需要控件信息时）

| 函数 | 用法 | 说明 |
| --- | --- | --- |
| `click(x, y)` | `click(540, 1200);` | 点坐标（带两个参数时是坐标点击） |
| `longClick(x, y)` | `longClick(540, 1200);` | 长按坐标 |
| `swipe(x1, y1, x2, y2, ms)` | `swipe(540, 1500, 540, 500, 300);` | 从起点滑到终点，最后是时长毫秒 |
| `press(x, y, ms)` | `press(540, 1200, 50);` | 按住指定时长，适合模拟手指按压 |

坐标点击受分辨率影响，换设备要重算；能找到控件就优先用第二节的控件点击。

## 四、截图与找色

截图相关的函数第一次运行会请求系统「屏幕录制」授权，同意后才有图像：

```javascript
if (!requestScreenCapture()) {
    toast("没有截图权限");
    exit();
}
let img = captureScreen();                    // 当前屏幕
let p = findColor(img, "#ff0000");            // 找颜色，返回坐标点或 null
if (p) click(p.x, p.y);
```

找图（`images.findImage`）需要先把模板图片存到本地再加载，适合固定界面的场景；找色对轻微改版更耐抗，两种方式按界面稳定性选。

## 五、设备与系统

```javascript
device.width;               // 屏幕宽，例如 1080
device.height;              // 屏幕高
device.keepScreenOn(60 * 60 * 1000);  // 让屏幕亮 1 小时（参数是毫秒）
toast(device.brand);        // 厂商名，用于给不同机型写分支
```

## 六、文件、网络与弹窗

```javascript
// 文件
files.write("/sdcard/记录.txt", "内容");
let s = files.read("/sdcard/记录.txt");

// 网络
let res = http.get("https://api.github.com");
log(res.body.string());

// 对话框（阻塞式，适合手动确认场景）
let ok = confirm("继续执行吗？");
let name = dialogs.input("请输入名称");
```

## 七、流程控制

```javascript
setInterval(() => {
    log("每 10 秒执行一次");
}, 10 * 1000);

events.observeKey();          // 监听按键，例如音量下键当作快捷开关
events.onKeyDown("volume_down", () => toast("检测到音量下键"));

// 停止当前脚本
exit();
```

## 八、官方文档地址

速查表只列了骨架，完整 API 与参数细节查官方文档（免费开源版 4.x）：

- 在线文档：[https://hyb1996.github.io/AutoJs-Docs/](https://hyb1996.github.io/AutoJs-Docs/)
- 项目仓库：[https://github.com/hyb1996/Auto.js](https://github.com/hyb1996/Auto.js)

写好脚本想做成独立应用或搬到电脑上开发，分别看 [打包与VSCode开发.md](打包与VSCode开发.md) 与 [脚本编写入门.md](脚本编写入门.md)；需要开启的前置权限（无障碍、截图）在 [无障碍服务开启教程.md](无障碍服务开启教程.md)。
