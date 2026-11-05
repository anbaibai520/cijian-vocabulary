# 词间 · 把单词用起来

个人英语词汇学习器，支持离线词库、主动回忆、词性变形、近义词辨析和语境练习。

**[下载 Android APK 和完整源码](https://github.com/anbaibai520/cijian-vocabulary/releases/latest)**

完整工程通过发行附件 `cijian-1.3.0-source.zip` 提供，包含网页、Android原生源码、构建脚本、测试和功能规划。请使用这个附件；GitHub自动生成的“Source code”归档对应本仓库文档目录，不是完整工程。解压完整工程后可以直接打开 `index.html`；电脑使用可选AI功能时运行 `启动AI学习器.cmd`。

## 已实现

- 英文优先录入，内置5万条 ECDICT 常用词；详细编辑折叠，缺项可后补。
- 词义、拼写、原句、造句、词性、近义词和新语境七类练习，各维度独立复习。
- 20词60道自编专项题；保留词族、搭配和中文辨析笔记。
- Tatoeba带署名例句、词典多义项选择、DeepSeek可选生成，先预览和审核再保存。
- 三种外观自由切换，本机数据存储和JSON备份。
- Android系统文件选择器、英语TTS、按需通知权限、RSA签名资源更新与启动失败资源回退。

## 使用说明

- [第二部分：例句、义项与DeepSeek配置](第二部分使用说明.md)
- [Android安装、权限及更新](Android安装与更新说明.md)
- [功能规划](功能规划.md)
- [开发扩展说明](开发扩展说明.md)
- [测试记录及验证边界](验证记录.md)

默认更新清单：`https://github.com/anbaibai520/cijian-vocabulary/releases/latest/download/latest.json`。网络不通时可下载签名ZIP后在手机导入。资源更新不代装APK。

密钥由使用者自行填写：电脑只保留在本机服务内存，Android经Keystore加密。API生成可能产生费用。个人词库、API密钥和签名私钥不在仓库及公开源码包中。

1.3.0已通过20项Node回归、9项Java更新校验和APK签名验证。尚需在用户手机验证安装、权限、TTS、文件操作、密钥加解密和更新回退；未用用户真实密钥调用付费API。
