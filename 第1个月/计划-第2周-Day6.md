# 第1个月｜第2周｜第6天

## 今日使命
- 实现 UI 状态管理：`ViewModel`、`SavedStateHandle`、`onSaveInstanceState`，应对配置变更与进程终止。
- 完成 `DetailActivity/Fragment` 的横竖屏切换、返回数据流程，验证资源适配（`layout-land`）。
- 联调周内功能，运行 `./gradlew assembleDebug`，整理周总结与问题清单。

## 时长分配（8h）
1. 80 min：AI 讲义（状态保存 + 配置变更 + ViewModel + SavedStateHandle）与情景题。
2. 150 min：为 Detail 页面添加 ViewModel/SavedStateHandle，处理屏幕旋转恢复。
3. 120 min：实现设置页/对话框，使用 `ActivityResultLauncher` 或 Fragment Result 回传数据。
4. 80 min：资源适配与测试矩阵（`layout-land`、`values-land`、字体/颜色）。
5. 90 min：联调与 `./gradlew assembleDebug`，捕捉日志，整理 `week2-summary.md`、`issues-week2.md`。
6. 40 min：复盘、周总结发布稿草拟。

## 知识块
- `ViewModel` 生命周期：跨 Activity 重建保持数据；`ViewModelProvider`.
- `SavedStateHandle`：存储轻量状态，配合导航组件或 `AbstractSavedStateViewModelFactory`.
- `onSaveInstanceState` vs `ViewModel`：前者应对进程终止，后者应对配置变更。
- 配置变更测试：旋转、分屏、系统字体/语言切换、深色模式。
- `gradlew assembleDebug`、APK 签名、Logcat + `adb install`。

## 讲义式要点
1. **ViewModel + SavedStateHandle**
   ```kotlin
   class DetailViewModel(private val state: SavedStateHandle) : ViewModel() {
       var uiState = state.getLiveData("uiState", DetailUiState())
       fun updateTitle(newTitle: String) {
           val updated = uiState.value?.copy(title = newTitle)
           state["uiState"] = updated
       }
   }
   ```
2. **onSaveInstanceState**
   ```kotlin
   override fun onSaveInstanceState(outState: Bundle) {
       super.onSaveInstanceState(outState)
       outState.putString(KEY_TITLE, binding.title.text.toString())
   }
   ```
3. **配置变更测试表**
   | 场景 | 期望行为 | 验证方式 |
   |------|----------|----------|
   | 旋转 | 数据保持 | 旋转→观察 UI |
   | 分屏 | 布局适配 | 分屏模式下运行 |
   | 深色模式 | 主题切换 | 设置中切换 Theme |
4. **Gradle 构建与日志**
   - `./gradlew assembleDebug --scan` 查看耗时任务。
   - 使用 `adb install app/build/outputs/apk/debug/app-debug.apk` 在真机安装。

## 实操任务
1. `DetailViewModel` + `SavedStateHandle`：保存课程详情状态（标题、进度、收藏）；支持恢复。
2. `DetailFragment` 支持 `layout-land` 布局；在 `notes/week2/state-handling.md` 绘制状态恢复流程。
3. 设置页或对话框：演示 `ActivityResultLauncher` / Fragment Result；写出回调日志。
4. `./gradlew assembleDebug`，记录构建耗时、错误、APK 位置；附截图或日志到 `notes/week2/build-log.md`。
5. `notes/week2-summary.md`：完成周总结（完成、问题、下周计划）；`issues-week2.md`：分类“IDE/生命周期/布局/状态管理”。

## AI Prompt 模板
1. `请讲解 ViewModel、SavedStateHandle、onSaveInstanceState 的区别与协同流程，并给出配置变更示例。`
2. `我在旋转屏幕后丢失数据，请根据以下代码帮我定位问题（可能涉及 SavedStateHandle/Bundle）。`
3. `请根据这份周成果草稿，输出一份结构化周报（完成/问题/计划）。`

## 产出清单
- `DetailViewModel`、状态恢复代码、`layout-land` 布局。
- `notes/week2/state-handling.md`、`notes/week2/build-log.md`、`notes/week2-summary.md`。
- `issues-week2.md` 更新、演示视频或截图。
- `notes/week2/day6-log.md`。

## 自查问题
1. 状态恢复是否覆盖：旋转、进程终止（杀进程）重启、返回再进入？
2. `ViewModel` 与 `SavedStateHandle` 是否正确提供（工厂/依赖注入）？
3. `onSaveInstanceState` 是否仅保存必要数据，Bundle 是否超量？
4. `layout-land` 是否真正生效，有无 UI 异常？
5. `assembleDebug` 是否成功，构建日志是否保存？
6. 周总结是否包含收获/问题/下周计划？

## 复盘模板
```
### 今日完成度
- 状态管理：✅ / ⚠️
- 配置变更测试：✅ / ⚠️
- 周总结 + 构建：✅ / ⚠️

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查：
- 结论/改进：

### 下周展望
- 布局进阶/RecyclerView 优化计划：
- 需要 AI 支持的知识点：
```
