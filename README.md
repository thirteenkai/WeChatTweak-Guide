# WeChatTweak 安装避坑指南 (Unofficial Guide)

> [!NOTE]
> **写在前面 / Motivation**
>
> 作为一个刚接触 macOS 的新用户，或者是想给朋友安利这款神器的人，你可能会在安装过程中遇到各种报错（比如权限不足、版本不兼容等）。
>
> 这份指南并不是官方文档，而是我在踩了无数坑（特别是帮朋友安装 Intel Mac 版本失败）后总结出的一份**少走弯路手册**。希望它能帮你一次点亮图标！

---

## 🙏 致谢 / Credits

* **核心工具**: 本项目完全基于大神 **@sunnyyoung** 的 [WeChatTweak](https://github.com/sunnyyoung/WeChatTweak)。
* **历史版本**: 感谢 **@zsbai** 维护的 [wechat-versions](https://github.com/zsbai/wechat-versions) 仓库，为降级提供了极大便利。

*本仓库仅作为安装流程的补充说明与避坑提示。*

---

## 🚫 兼容性警告 (最重要的事)

在开始之前，请务必确认你的电脑芯片类型：

> [!CAUTION]
> **Intel Mac 用户请劝退**
>
> 目前最新版本的 WeChatTweak (尝鲜版) **不再支持 Intel (x86_64) 架构**。
> 如果你是 Intel 机型（2020 年及之前的 Mac），安装后**功能将无法生效**（防撤回失败、无菜单）。建议寻找旧版或放弃折腾。
<!-- -->
> [!TIP]
> **Apple Silicon (M1/M2/M3/M4) 用户**
>
> 本指南专为你们准备，请放心食用。✅

---

## 🛠 安装步骤

### 1. 准备工作

* **网络环境**：请确保你的终端 (Terminal) 可以正常访问 GitHub (建议开启魔法/增强模式)。
* **环境依赖**：只需安装 **Homebrew** (它会自动帮你处理 Xcode CLI 等开发工具)。
    *(如果你连 Homebrew 也没装，请复制下面的官网命令)*

    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

### 2. 安装 WeChatTweak (通过 Homebrew)

打开终端 (Terminal)，直接运行：

```bash
# 1. 添加仓库并下载
brew install sunnyyoung/tap/wechattweak

# 2. 注入插件 (关键步骤)
# 注意：运行前请彻底退出微信 (Command + Q)
sudo wechattweak patch
```

* **为什么要加 `sudo`？**
    官方文档主要是 `wechattweak patch`，但很多时候会因为 `/Applications` 文件夹权限问题报错。为了保证一次成功，建议加上 `sudo`，输入开机密码即可。

### 3. (可选) 签名修复

虽然现在的版本通常自动处理签名，但如果你打开微信**闪退**，请运行这行命令“压惊”：

```bash
sudo codesign --force --deep --sign - /Applications/WeChat.app
```

---

## ✅ 验证是否成功

对于最新版本的插件 (尝鲜版)，可能不会在界面上显示明显的菜单项。最直接的验证方式是**功能测试**：

1. **重启微信**。
2. 找一位好友配合，让他给你发一条消息并**撤回**。
3. 观察效果：
    * 🎉 **成功**：**原消息依然显示**在屏幕上（注意：可能不会有任何“对方撤回了一条消息”的系统提示，表现得就像对方从未撤回过一样）。
    * ❌ **失败**：消息直接消失了 (说明插件未生效，请检查是否为 Intel 机型)。

---

## ❓ 常见问题 (FAQ)

**Q: 插件失效了，我该怎么排查？**
A: 首先请检查你的 WeChat 版本是否在官方支持列表中。

1. **查询支持版本**：在终端运行 `wechattweak versions`，它会列出当前插件版本支持的所有微信 Build 号。
2. **查询当前版本**：打开微信 → 关于微信，对比版本号（或在终端运行 `defaults read /Applications/WeChat.app/Contents/Info CFBundleVersion` 查看精确 Build 号）。
3. **下载旧版微信**：如果当前版本不支持，请前往历史版本仓库下载兼容版本：
    👉 **[zsbai/wechat-versions/releases](https://github.com/zsbai/wechat-versions/releases)**

**Q: 我按照教程做了，为什么 Intel Mac 还是不行？**
A: 还是那句话，新版架构确实不支持了。这不是 Bug，是由于底层实现变更导致的硬件门槛。
