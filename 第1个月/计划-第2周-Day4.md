# 第1个月｜第2周｜第4天

## 今日使命
- 启用 ViewBinding，优化 Activity/Fragment/Adapter 中的视图引用，形成基类或扩展函数。
- 实现课程列表页（RecyclerView + Adapter + 点击事件），并应用 Day3 的布局组件。
- 编写 ViewBinding 使用手册、RecyclerView 测试或日志，准备第5天的 Fragment 集成。

## 时长分配（8h）
1. 70 min：AI 讲义（ViewBinding + RecyclerView 架构）与案例。
2. 150 min：启用 ViewBinding、封装 `BindingActivity`/`BindingFragment`。
3. 150 min：实现 `CourseAdapter`、`ListFragment/ListActivity`，完成点击跳转。
4. 80 min：编写 `notes/week2/viewbinding-cheatsheet.md`、`notes/week2/adapter-spec.md`。
5. 40 min：编写简单测试/日志验证 DiffUtil（如使用 `DiffUtil.calculateDiff`）。
6. 30 min：复盘与问题清单。

## 知识块
- ViewBinding 启用：`buildFeatures.viewBinding = true`、生成的 `ActivityMainBinding`。
- Activity 中的 ViewBinding 生命周期管理：`private lateinit var binding`、`setContentView(binding.root)`。
- Fragment 中的 ViewBinding：`_binding`、`onDestroyView` 清理。
- RecyclerView 组件：`RecyclerView`、`Adapter`、`ViewHolder`、`DiffUtil`（进阶）、`ListAdapter`。
- 点击事件封装：接口回调、Lambda、Navigation。

## 讲义式要点
1. **ViewBinding 在 Activity 中**
   ```kotlin
   class MainActivity : AppCompatActivity() {
       private lateinit var binding: ActivityMainBinding
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           binding = ActivityMainBinding.inflate(layoutInflater)
           setContentView(binding.root)
       }
   }
   ```
2. **Fragment 中的 `_binding` 模式**
   ```kotlin
   private var _binding: FragmentDashboardBinding? = null
   private val binding get() = _binding!!
   override fun onCreateView(...) = FragmentDashboardBinding.inflate(inflater, container, false).also { _binding = it }.root
   override fun onDestroyView() { super.onDestroyView(); _binding = null }
   ```
3. **RecyclerView Adapter + ViewBinding**
   ```kotlin
   class CourseAdapter(
       private val onClick: (Course) -> Unit
   ) : ListAdapter<Course, CourseViewHolder>(CourseDiff) {
       override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): CourseViewHolder {
           val binding = ItemCourseBinding.inflate(LayoutInflater.from(parent.context), parent, false)
           return CourseViewHolder(binding)
       }
       override fun onBindViewHolder(holder: CourseViewHolder, position: Int) = holder.bind(getItem(position))
   }
   ```
4. **DiffUtil（进阶）**
   ```kotlin
   object CourseDiff : DiffUtil.ItemCallback<Course>() {
       override fun areItemsTheSame(old: Course, new: Course) = old.id == new.id
       override fun areContentsTheSame(old: Course, new: Course) = old == new
   }
   ```

## 实操任务
1. 在 `app/week2-sample/build.gradle.kts` 中启用 ViewBinding；创建 `BindingActivity` 基类封装 `binding` 初始化。
2. `MainActivity` 使用 ViewBinding，展示课程列表入口；`DashboardFragment` 预留容器。
3. `CourseAdapter` + `RecyclerView`：使用 Day3 的 `item_course.xml`；点击后通过 Intent 跳转 `DetailActivity`，传递课程 ID。
4. `notes/week2/viewbinding-cheatsheet.md`：覆盖 Activity/Fragment/Adapter 写法、生命周期注意事项、常见错误（如 `_binding` 泄漏）。
5. `notes/week2/adapter-spec.md`：记录数据模型、DiffUtil 策略、点击事件协议、测试记录。

## AI Prompt 模板
1. `请讲解 ViewBinding 在 Activity/Fragment/Adapter 中的使用方式，解释如何避免内存泄漏，并附 Kotlin 示例。`
2. `以下是我的 RecyclerView 需求，请生成 Adapter + DiffUtil 代码骨架，并指出点击事件的封装方式。`
3. `请审查这段 ViewBinding 代码，确认是否存在生命周期泄漏或 NPE 风险。`

## 产出清单
- `BindingActivity`、`MainActivity`、`CourseAdapter`、`item_course.xml` 整合版本。
- `notes/week2/viewbinding-cheatsheet.md`、`notes/week2/adapter-spec.md`。
- `assets/screenshots/day4-list.png`。
- `notes/week2/day4-log.md`。

## 自查问题
1. ViewBinding 是否在所有 Activity/Fragment 中启用并正确释放？
2. RecyclerView 的 Adapter 是否使用 ViewBinding、DiffUtil？点击事件是否解耦？
3. 列表加载数据是否放在 ViewModel/Repository（如暂时使用 Fake 数据）？
4. 日志/错误是否记录在 `issues-week2.md`？
5. 是否为 Day5 的 Fragment 整合预留接口？

## 复盘模板
```
### 今日完成度
- ViewBinding 集成：✅ / ⚠️
- RecyclerView 列表：✅ / ⚠️
- 文档/截图：✅ / ⚠️

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查：
- 结论：

### 明日准备
- Fragment/导航关键点：
- 需要 AI 协助的问题：
```
