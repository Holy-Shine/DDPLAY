# DDPLAY：弹弹play 坚果投影适配

基于 [DanDanPlayForAndroid](https://github.com/xyoye/DanDanPlayForAndroid) 4.1.4 源码，为用户的坚果 N1S Pro 评估并逐步开展适配。本仓库为个人私有开发仓库。

当前阶段：源码分析及方案讨论；尚未修改应用逻辑，尚未验证编译或真机安装。请先阅读 [AGENTS.md](AGENTS.md) 和 [项目状态](docs/project-status.md)。

## 在另一台电脑继续开发

1. 配置 Git 和该电脑自己的 GitHub 认证，并确保账号有此私有仓库的访问权限。
2. 克隆仓库：

   ```sh
   git clone https://github.com/Holy-Shine/DDPLAY.git
   cd DDPLAY
   ```

3. 在 Codex 中打开整个 `DDPLAY` 目录，使根目录 `AGENTS.md` 生效。接续工作前阅读项目状态文档，并给出这次具体任务。
4. Android Studio 应打开 `DanDanPlayForAndroid-dandanplay-v4.1.4` 子目录，而非外层 Git 根目录。
5. 换电脑前提交并推送需要同步的变更；另一台电脑工作区干净时使用 `git pull --ff-only` 获取更新。不要同时在两台电脑修改同一分支而不先同步。

## 构建准备

源码配置：Gradle Wrapper 7.4、Android Gradle Plugin 7.3.1、Kotlin 插件 1.7.10、compileSdk 33、minSdk 21、targetSdk 29。

建议先使用 JDK 11 和 Android SDK Platform 33 恢复原工程构建，暂不升级工具链。该组合为根据项目配置给出的环境建议，尚未在本任务中验证成功。使用仓库内 Gradle Wrapper，不依赖全局 Gradle。

每台电脑独立配置 Android SDK；可由 Android Studio 生成 `local.properties`，不要提交含个人 SDK 路径的文件。构建需要网络下载 Gradle 和 Maven 依赖。

在 Android 工程子目录执行：

```powershell
# Windows PowerShell
.\gradlew.bat --version
.\gradlew.bat :app:assembleDebug
```

```sh
# macOS / Linux
sh ./gradlew --version
sh ./gradlew :app:assembleDebug
```

源码通过自定义 `SignConfig.kt` 从 `gradle/assemble` 读取签名配置。上游默认调试配置会引用 `debug.jks`；该文件被原有 `.gitignore` 排除，不在 Git 历史内，因此新克隆不能假定直接构建成功。开发时需另行准备调试 keystore，通过已忽略的 `gradle/assemble/keystore.properties` 指定 `KEY_LOCATION`、`KEYSTORE_PASS`、`ALIAS_NAME`、`ALIAS_PASS`。不要把个人配置和签名文件加入仓库。

发布构建依赖独立发布签名及环境变量；尚未建立本项目发布流程。多台电脑使用不同调试签名时，已有安装可能无法覆盖升级，应先确认签名安排。

## 来源与目录

- `DanDanPlayForAndroid-dandanplay-v4.1.4/`：原 Android 多模块源码，保持导入目录结构。
- `AGENTS.md`：所有电脑上的 Codex 开发约定。
- `docs/project-status.md`：当前状态、初步方案与待确认事项。
- 上游说明与许可分别见 [原 README](DanDanPlayForAndroid-dandanplay-v4.1.4/README.md) 和 [LICENSE](DanDanPlayForAndroid-dandanplay-v4.1.4/LICENSE)。保留原作者署名及第三方许可。

源码、项目文档和 Git 历史通过本私有仓库同步；聊天上下文、SDK、构建缓存、凭据及签名文件不作为仓库交接材料。
