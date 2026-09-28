# PakePlus 云端打包的 GitHub Token 获取与配置

> 本篇讲云端打包为什么要 GitHub Token、怎么用授权登录一步拿到，以及手动创建 Token 时该勾哪些权限、怎么填回客户端。
> **相关文档**：[手机上把网页打包成APK的步骤.md](手机上把网页打包成APK的步骤.md) · [本地打包和云端打包的区别.md](本地打包和云端打包的区别.md) · [常见问题与报错排查.md](常见问题与报错排查.md)

---

> [!IMPORTANT]
> **PakePlus 安装文件资源（夸克网盘）**：[https://pan.quark.cn/s/4db1190dc6e1](https://pan.quark.cn/s/4db1190dc6e1)

---

## 一、什么情况下需要它

云端打包的编译过程跑在 GitHub 的服务器上：你的项目文件存到你自己的 GitHub 仓库里，编译也由 GitHub 完成，客户端只是替你操作这些事，所以需要一把有对应权限的钥匙——就是 GitHub Token。

只走本地打包的话，整个流程不碰 GitHub，**不需要 Token**，这篇可以跳过。

先说一个官方的交代：用 Token 打包，会自动复制（fork）PakePlus 及其子仓库到你的账号下，并统计编译成功还是失败，用于改进项目。Token 只保存在你本地，不会上传到别的服务器。

## 二、最省事的路：GitHub 授权登录

客户端首页提供 GitHub 授权登录入口，点它、按页面提示完成授权，Token 就自动拿到并填好了，不需要看后面的手动步骤。没有 GitHub 账号的话，先去 `https://github.com/` 注册一个。

## 三、手动创建 Token 的完整步骤

想手动创建（比如授权登录报错时），走 classic Token，只需要勾三个权限：

1. 登录 GitHub，点右上角头像 → `Settings`。
2. 左侧菜单拉到最底，点 `Developer settings`。
3. 点 `Personal access tokens` → `Tokens (classic)`，也可以直接打开 `https://github.com/settings/tokens`。
4. 点 `Generate new token (classic)`。
5. 勾选 **repo**、**workflow**、**user** 三个权限，其他不用动。
6. 点生成，**立刻复制保存**——Token 只显示这一次，离开页面就再也看不到了。

三条权限各管一件事：`repo` 负责仓库的增删改查和 fork，`workflow` 负责跑编译，`user` 用于读取账号信息。

## 四、填回客户端并测试

1. 回到 PakePlus，点首页右上角的设置按钮，把 Token 粘贴进去。
2. 点**测试**，会校验 Token 是否有效并做初始化，网络顺畅时二十秒左右出结果。
3. 提示 Token 可用就成功了，右上角会出现你的 GitHub 头像；提示不可用或一直转圈，多半是 Token 复制错了、Token 已过期或被撤销、或网络不通，重新生成再试，排查细节见 [常见问题与报错排查.md](常见问题与报错排查.md)。

## 五、如果用的是细粒度 Token

GitHub 现在也提供细粒度（fine-grained）Token，官方指南对应的权限是这几项，创建时照着勾：

| 权限 | 用途 |
| --- | --- |
| All repositories | 要 fork 原始模板仓库，所以范围选全部仓库 |
| Actions | 操作 GitHub Actions 完成打包编译 |
| Administration | 对仓库进行 fork 和文件管理 |
| Contents | 对 PakePlus 仓库做添加、删除、修改、查找 |
| Workflows | 用来编译打包你的软件 |

（`Metadata` 是 GitHub 自动附带勾选的，不用管。）手动配置比 classic 三勾麻烦，拿不准就用 classic。

## 六、保管与清理

- Token 如果设置了有效期，过期后要重新生成一个。
- 退出登录会**删除并清空本地所有记录**，包括 Token 和全部项目数据，点之前想清楚。
- 打包过程会在你的 GitHub 账号下产生仓库（模板的复制品），不想要了可以删：打开那个仓库 → `Settings` → 拉到页面最底 `Delete this repository` → 按提示输入仓库名确认删除。
