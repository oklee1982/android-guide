# 第1个月｜第2周｜第2天

## 今日使命
- 掌握 Activity 生命周期、Intent（显式/隐式）、`ActivityResultLauncher`。
- 在 Demo 中实现 `MainActivity` → `DetailActivity`，完成参数传递、结果返回、生命周期日志。
- 输出生命周期图、Intent 流程文档，确保 README 含运行说明。

## 时长分配（8h）
1. 70 min：AI 讲义（生命周期 + Intent）与小测。
2. 150 min：实现两个 Activity、Intent + Result API、日志封装。
3. 120 min：绘制生命周期图、截图、README 更新。
4. 80 min：异常与配置变更实验（横竖屏切换、任务切换）。
5. 60 min：整理 `notes/week2/lifecycle-log.md`、`notes/week2/intent-spec.md`。
6. 40 min：复盘与问题清单。

## 知识块
- Activity 生命周期：`onCreate`→`onStart`→`onResume`→`onPause`→`onStop`→`onDestroy`，以及 `onRestart`。
- Intent 类型：显式、隐式、`bundle`、`Parcelable`；`PendingIntent` 概念。
- `ActivityResultLauncher`、`ActivityResultContract`、`registerForActivityResult`。
- 日志与调试：`Log.d`、`LifecycleObserver`、Logcat 过滤。

## 讲义式要点
1. **生命周期回调顺序**
   ```kotlin
   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) { super.onCreate(savedInstanceState); log("onCreate") }
       override fun onStart() { super.onStart(); log("onStart") }
       override fun onResume() { super.onResume(); log("onResume") }
       override fun onPause() { log("onPause"); super.onPause() }
       override fun onStop() { log("onStop"); super.onStop() }
       override fun onDestroy() { log("onDestroy"); super.onDestroy() }
   }
   ```
   - 前台→后台：`onPause`→`onStop`；返回前台 `onRestart`→`onStart`→`onResume`。
2. **Intent 传参**
   ```kotlin
   val intent = Intent(this, DetailActivity::class.java).apply {
       putExtra(EXTRA_ID, itemId)
       putExtra(EXTRA_TITLE, titleInput.text.toString())
   }
   startActivity(intent)
   ```
   - 使用常量 Key；复杂对象实现 `Parcelable` 或 `Serializable`。
3. **ActivityResultLauncher**
   ```kotlin
   private val editLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
       if (result.resultCode == RESULT_OK) {
           val data = result.data?.getStringExtra(EXTRA_NOTE)
           updateNote(data)
       }
   }

   fun openEditor() {
       editLauncher.launch(Intent(this, EditActivity::class.java))
   }
   ```
   - 避免过时的 `startActivityForResult`。
4. **生命周期日志封装**
   ```kotlin
   interface LifecycleLogger {
       fun log(owner: String, event: String) = Log.d("Lifecycle", "$owner -> $event")
   }
   ```
   - 统一日志格式，方便复制到 `notes/week2/lifecycle-log.md`。

## 实操任务
1. `MainActivity` 显示列表/按钮，点击跳转 `DetailActivity`，传递内容 ID 与标题。
2. `DetailActivity` 支持“编辑/完成”操作，使用 `ActivityResultLauncher` 返回结果给 `MainActivity`；更新 UI。
3. 日志：实现 `LifecycleLogger` 接口，将回调写入 `notes/week2/lifecycle-log.md`（含横竖屏切换、Home 键场景）。
4. `notes/week2/intent-spec.md`：描述 Intent 字段、来源、目标、错误处理（空值、缺少 extras）。
5. 更新 README：添加运行步骤、Intent 演示 GIF 或截图。

## AI Prompt 模板
1. `请生成 Activity 生命周期 vs 回调用途的对照表，并给出 2 个配置更改情景题。`
2. `以下是我实现的 Intent 代码，请帮我做代码审查，重点关注 null 安全、常量命名、结果回调。`
3. `我需要在 Demo 中演示 ActivityResultLauncher，请提供最小可运行示例并解释 Contract 的原理。`

## 产出清单
- `MainActivity`、`DetailActivity` 实现与日志。
- `notes/week2/lifecycle-log.md`、`notes/week2/intent-spec.md`。
- README 更新、截图或 GIF。
- `notes/week2/day2-log.md`。

## 自查问题
1. 生命周期日志是否覆盖“首次启动、Home 键、屏幕旋转、返回栈”四种情况？
2. Intent Extras 是否集中定义常量？空值如何处理？
3. 是否已使用 `ActivityResultLauncher` 替换旧 API？
4. README 是否描述运行步骤与注意事项？
5. 所有问题是否记录在 `issues-week2.md`？

## 复盘模板
```
### 今日完成度
- Activity/Intent 功能：✅ / ⚠️
- 生命周期日志：✅ / ⚠️
- README/文档：✅ / ⚠️

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查：
- 结论/改进：

### 明日准备
- View 组件要点：
- 待问 AI 的问题：
```
