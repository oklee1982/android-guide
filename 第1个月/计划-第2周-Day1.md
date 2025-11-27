# 第1个月｜第2周｜第1天

## 今日使命
- 完成 Android Studio、SDK、AVD、真机调试等环境检查，并理解 Gradle 项目结构。
- 在仓库中创建 `app/week2-sample/` 工程，熟悉 `build.gradle.kts`、Manifest、资源目录。
- 整理 IDE 与构建流程笔记，形成可复用的环境指导文档。

## 时长分配（8h）
1. 60 min：AI 讲义（IDE 面板 + Gradle 基础）及小测。
2. 150 min：创建工程、配置包结构、同步 Gradle、运行示例 App。
3. 120 min：记录环境与构建日志，排查至少 2 个潜在问题（例如 Gradle Sync 失败）。
4. 60 min：熟悉 Logcat、Build Analyzer、Gradle Task 图。
5. 60 min：撰写 `notes/week2/ide-setup.md`、`notes/week2/env.md`。
6. 30 min：日终复盘与问题清单更新。

## 知识块
- Android Studio UI：Project、Gradle、Logcat、Layout Inspector 面板。
- Gradle 项目结构：settings.gradle、根/模块级 `build.gradle.kts`、`gradle.properties`。
- Manifest 与资源目录结构：`AndroidManifest.xml`、`res/values`、`res/layout`。
- Gradle 同步、构建流程与常见错误排查。

## 讲义式要点
1. **Android Studio 面板速览**
   - Project：切换 Android/Project 视图了解模块目录。
   - Gradle：查看 Tasks、Dependencies；可直接运行 assemble 任务（进阶：可选）。
   - Logcat：配置过滤器、设备筛选、Tag 与 Level。
2. **Gradle 文件拆解**
   - `settings.gradle.kts`：声明包含的模块、插件管理。
   - 根级 `build.gradle.kts`：版本插件、仓库、全局配置。
   - 应用模块 `build.gradle.kts`：`compileSdk`、`defaultConfig`、`buildTypes`、`buildFeatures.viewBinding`.
3. **Manifest / 资源目录**
   - Manifest 结构：包名、`application`、`activity`、`intent-filter`。
   - `res/values` 下的 colors、strings、themes；新建 `values-night` 实现深色支持。
4. **Gradle Sync & 构建排错**
   - 常见报错：JDK 版本不匹配、SDK 未安装、代理导致依赖下载失败。
   - 使用 Build Analyzer 查看耗时任务；Gradle Wrapper 版本控制在 `gradle/wrapper/gradle-wrapper.properties`.

## 实操任务
1. `app/week2-sample/`：通过 Empty Activity 模板创建工程，包名建议 `com.example.week2`.
2. 调整包结构：`ui/main`、`ui/detail`、`data`、`core/common`，并同步 Gradle。
3. `notes/week2/env.md`：记录 JDK、Android Studio 版本、SDK Platform、AVD/真机信息。
4. `notes/week2/ide-setup.md`：整理 IDE 面板说明、常用快捷键、Gradle 构建步骤。
5. 截图：Android Studio 主界面、Project 结构、模拟器运行画面，保存到 `assets/screenshots/day1-*.png`.

## AI Prompt 模板
1. `请用表格讲解 Android Studio 的 Project/Gradle/Logcat 面板功能，并附 3 个常见错误与排查步骤。`
2. `以下是我的 build.gradle.kts，请指出 compileSdk/minSdk/buildFeatures 的最佳实践设置，并解释如何启用 ViewBinding。`
3. `Gradle Sync 报错日志如下，请帮我分析原因并给出排查顺序。`

## 产出清单
- `app/week2-sample/` 工程（可运行）。
- `notes/week2/env.md`、`notes/week2/ide-setup.md`。
- `assets/screenshots/day1-ide.png`、`day1-emulator.png` 等截图。
- 日志 `notes/week2/day1-log.md`：记录已知/未知/验证点。

## 自查问题
1. Gradle Wrapper、JDK、Android Studio 版本是否记录？是否与命令行一致？
2. `compileSdk`、`minSdk`、`targetSdk` 的取值依据是什么？
3. 项目包结构是否符合 `ui/`、`data/` 等规划？
4. 能否在模拟器与真机各运行一次 Demo？
5. 发生错误时是否在 `notes/week2/ide-setup.md` 中记录原因与解决方案？

## 复盘模板
```
### 今日完成度
- IDE/SDK 配置：✅ / ⚠️
- week2-sample 工程：✅ / ⚠️
- 文档/截图：✅ / ⚠️

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查步骤：
- 解决方案/后续计划：

### 明日准备
- Activity/Intent 问题：
- 需要 AI 协助的点：
```
