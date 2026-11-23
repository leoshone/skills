# Skills

可复用的 AI agent skills（通用方法沉淀），跨项目共享。

> **AI 使用约定**：接手依赖本仓库 skill 的项目（如 zhihu-desk2mob）时，
> **开始开发或测试前**先把所需 skill 目录复制为**项目级 skill**（如 WorkBuddy 的
> `{workspace}/.workbuddy/skills/`），而不是每次现场翻这个仓库——项目级 skill 会被
> AI 工具自动发现与加载，仓库副本只是上游源头。

| Skill | 用途 |
|---|---|
| [avd-boot](avd-boot/) | Windows 上启动本机 Android 模拟器（AVD）：计划任务脱离调用会话启动，避免模拟器被连带杀死 |
| [avd-install-apk](avd-install-apk/) | 在 Android 模拟器（AVD）安装第三方 APK/APKS/XAPK：下载源选择、split 包安装、踩坑速查 |
| [browser-extension-install](browser-extension-install/) | Windows Chromium 系浏览器（Cent/Turbo 等保留 MV2 的国产内核）安装 uBO、暴力猴等扩展 + CDP 自动化测试 |
| [violentmonkey-script-install](violentmonkey-script-install/) | 通过 CDP 自动化在浏览器（桌面/安卓 Kiwi 等）安装暴力猴用户脚本（.user.js） |

## 使用

把所需 skill 目录复制到项目级（AI 在新环境接手项目时，应先确认这些 skill 已就位）。
