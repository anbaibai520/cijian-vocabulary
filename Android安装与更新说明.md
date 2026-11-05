# Android 安装与签名更新

版本 1.3.0，包名 `cn.cijian.vocabulary`，最低 Android 8.0（API 26），目标 API 35。APK 已使用本机发布密钥签名；适合个人安装使用。

## 安装与数据

从 GitHub Releases 或本文件夹的 `apk/cijian-1.3.0.apk` 获取安装包，在手机上打开。系统可能要求允许本次所用浏览器或文件管理器安装应用；应用自身不会申请代装 APK 的权限。更新原生 APK 时保留包名和签名，采用覆盖安装，先导出 JSON 备份。

电脑浏览器与 APK 的词库各自保存在本机，不会自动同步。换设备时在旧端导出 JSON，再在新端导入。卸载、清除应用数据会删除本机词库；请定期导出。备份不是存放在 GitHub 的应用资源包。

## 权限按需使用

| 能力 | 权限与触发方式 |
| --- | --- |
| 联网词典、更新 | INTERNET 是普通安装权限，无运行时弹窗。联网补全可在词条编辑中关闭；自动更新检查默认关闭，手动检查才访问更新地址 |
| 每日复习提醒 | 默认关闭。点击手机设置启用时先解释用途，Android 13+ 再请求 POST_NOTIFICATIONS；拒绝不影响学习 |
| 重启后恢复提醒 | RECEIVE_BOOT_COMPLETED 只用于恢复用户已开启的提醒；约20点发送，系统省电可能延迟 |
| 导入、导出 | 系统文件选择器，仅访问你选择的文件，无全盘存储权限 |
| 发音 | 系统 TTS，不申请麦克风；语音引擎及英语语音包需设备可用 |

不申请相机、定位、通讯录、麦克风、安装其他应用或精确闹钟权限。应用不内置广告、分析或账号服务。词条联网补全向 Free Dictionary API / Datamuse / Tatoeba 发送当前查询的英文词，服务可能接收IP等常规连接信息；个人笔记、完整词库和学习记录不会随查询上传。用户配置并确认使用DeepSeek时，额外发送当前中文义和可选原文例句；密钥由Android Keystore加密存储，见第二部分说明。

## GitHub 方案

公开仓库：https://github.com/anbaibai520/cijian-vocabulary

发行版同时放置 APK、完整源码 ZIP、签名资源包 `web-10301.zip`、`latest.json` 和 APK 校验值。默认清单地址：

`https://github.com/anbaibai520/cijian-vocabulary/releases/latest/download/latest.json`

GitHub 网络在部分地区可能不稳定。检查失败不影响离线学习，可在电脑下载签名 ZIP 后使用“手机设置与更新 → 从文件导入签名更新包”。也可配置自己控制的 HTTPS 静态站点，保持相同清单格式和签名。

资源包适合界面、词库和练习内容更新；新增系统权限、原生能力或提高最低原生版本时发布新 APK。没有自动安装 APK 功能。

## 手机上的更新过程

手机设置与更新 → 手动检查 → 展示版本说明 → 用户允许下载 → 验证内置公钥对应的 RSA 签名、全部文件 SHA-256、文件白名单和版本号 → 再次确认应用 → 重载界面。也可明确开启“启动时每日检查”，每天最多检查一次，只提示，不自动应用。

更新保持固定 HTTPS 本地来源，所以现有 localStorage 数据保留。更新前另存一份应用私有数据快照；启动15秒未就绪会回退资源，上次未完成启动则下次启动回退。可在设置恢复 APK 内置资源。资源回退不会自动回滚数据结构；未来更改数据格式必须写兼容迁移并预先导出备份。内置快照不等同于用户可见的完整备份。

首次资源包10301用于验证发布渠道，网页内容与内置10300一致。更新包只允许16个固定网页文件，不接收DEX、原生库、任意路径或额外文件。更新清单只是发现入口，包内签名清单才是可信依据。

## 开发构建与后续发布

需要 Node、Python、JDK17、Android SDK（platforms;android-35、build-tools;35.0.0）。SDK条款应由构建者自行阅读同意。无需 Gradle。

```text
node tools/android-build.cjs <JDK目录> <Android SDK目录>
node --test tests/core.test.cjs tests/providers.test.cjs tests/enrichment.test.cjs
```

生成更新前先修改、验证网页，增加资源版本号（必须大于已发布版本），设置 Python 路径环境变量 `CIJIAN_PYTHON`（可用PATH中的python则不需设置），然后：

```text
node tools/make-update.cjs 10302 "本次更新说明" https://github.com/anbaibai520/cijian-vocabulary/releases/download/v1.3.1/web-10302.zip
```

创建对应新 Release，先上传并验证 ZIP，再上传同次生成的 latest.json。把该 Release 标记为最新。保留旧版本下载。仅修改清单不能绕过包内签名；地址必须HTTPS。

`.signing` 保存 APK 密钥、其密码文件和资源更新私钥。请离线备份到你控制的安全位置，不要上传 GitHub、发给他人或放入公开 ZIP。丢失 APK 密钥将无法覆盖升级现有安装；丢失更新私钥则旧 APK 不再信任新资源包。公开的 `android/update-public.pem` 可上传。首次构建缺少私钥会新建一组，不能当作原发行版继续更新。

源码包经过显式白名单导出，不含 `.signing`、构建目录、个人词库或历史备份。目录结构为网页入口、android 原生源码、tools 构建/发布脚本、tests 回归测试，保留后续扩展接口。
