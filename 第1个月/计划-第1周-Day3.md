# 第1个月｜第1周｜第3天

## 今日使命
- 掌握 Kotlin 面向对象语法：类、构造函数、继承、接口、`abstract`、`object`、`companion object`、`data class`、`sealed class`。
- 将 Day2 的待办 CLI 重构到 OOP 架构：领域模型、仓库、命令解析器。
- 形成 UML 草图和“重构前后差异”表，理解可测试性改进点。

## 时长分配（8h）
1. 60 min：AI 讲义 + data class / sealed class 练习。
2. 150 min：OOP 重构（建模、接口抽象、编写类）。
3. 120 min：为核心类编写单元测试与依赖注入（构造注入）。
4. 60 min：向 AI 查询“Classes & Inheritance”要点并整理讲义摘要（代替官方文档）。
5. 60 min：错题与重构复盘，绘制类图。
6. 30 min：面试题速答（`data class` 生成方法、`object` vs `companion`）。

## 知识块
- 基础 OOP：主/次构造、`init`、访问控制、`lateinit`。
- 数据建模：`data class` 自动生成的 `copy/equals/hashCode`。
- 继承与多态：`open`、`final`、`sealed` 限制继承。
- 单例实现：`object`、伴生对象、工厂方法。

## 讲义式要点

### 1. 类与构造、`init` 代码块
```kotlin
class Todo(
    val id: Int,
    var title: String,
    var completed: Boolean = false
) {
    init {
        require(title.isNotBlank()) { "标题不能为空" }
    }

    fun markDone() {
        completed = true
    }
}
```
- 主构造函数紧跟类名；可配合 `init` 做统一校验。
- 次构造函数通过 `constructor(...) : this(...)` 复用主构造逻辑。
- 属性若需要延迟初始化可用 `lateinit var`，但 prefer 构造注入。

### 2. Data class 的“免费函数”
```kotlin
data class TodoItem(
    val id: Int,
    val title: String,
    val completed: Boolean
)

val original = TodoItem(1, "阅读", false)
val updated = original.copy(completed = true)
println(original == updated.copy(completed = false)) // false
```
- 自动生成 `equals/hashCode/toString/copy/componentN`，适合表示纯数据。
- `copy` 常用于“不可变 + 修改字段”的场景；替代手写 setter。

### 3. Sealed class + `when` 穷举
```kotlin
sealed class Command {
    data class Add(val title: String) : Command()
    data class Complete(val id: Int) : Command()
    object List : Command()
    object Exit : Command()
}

fun handle(command: Command): String = when (command) {
    is Command.Add -> "新增 ${command.title}"
    is Command.Complete -> "完成 #${command.id}"
    Command.List -> "展示任务"
    Command.Exit -> "退出"
}
```
- 同一文件中列出所有子类，`when` 自动强制处理所有分支，不需要 `else`。
- 适合“有限状态/命令”建模。

### 4. 抽象类、接口与依赖注入
```kotlin
interface TodoRepository {
    fun add(item: TodoItem): TodoItem
    fun list(): List<TodoItem>
}

class InMemoryTodoRepository : TodoRepository {
    private val store = mutableListOf<TodoItem>()
    override fun add(item: TodoItem): TodoItem {
        store += item
        return item
    }
    override fun list(): List<TodoItem> = store.toList()
}

class TodoService(private val repo: TodoRepository) {
    fun create(title: String) =
        repo.add(TodoItem(storeId(), title, false))
}
```
- 通过接口限制依赖；`TodoService` 只感知 `TodoRepository`，便于测试注入 Fake。
- `abstract` 关键字定义抽象类，可实现部分共用逻辑 + 留下抽象函数。

### 5. `object` 单例与伴生对象
```kotlin
object Logger {
    fun info(msg: String) = println("[INFO] $msg")
}

class IdGenerator private constructor() {
    companion object {
        private var current = 0
        fun next(): Int = ++current
    }
}
```
- `object` 声明线程安全单例；可用作工具类、状态管理。
- `companion object` 内的函数可当作静态方法使用：`IdGenerator.next()`.
- 当需要可替换的依赖时，避免硬编码单例，可通过接口+注入。

### 6. 重构清单
1. **拆分职责**：命令解析、业务操作、存储分离。
2. **依赖方向**：高层（Service）依赖抽象（Repository），实现类放底层。
3. **可测试性**：为 Service 提供 fake repository，避免真实 IO。

## 实操任务
1. `code/kotlin-week1/domain/Todo.kt`、`TodoRepository.kt`、`CommandParser.kt` 等文件结构化。
2. `notes/day3-design.md`：
   - 类图或文本描述，包括依赖关系。
   - 重构前后差异（可测试性、可扩展性、可读性）。
3. 为 `CommandParser` 与 `TodoRepository` 各写 3 条测试，演示依赖注入。
4. 引入简单日志工具/扩展函数记录操作（可自定义 `Logger` 接口）。

## AI Prompt 模板
- 知识点：`请结合示例讲解 Kotlin data class、sealed class、object/companion object 的区别与使用场景。`
- 设计辅助：`根据以下需求，帮我列出 Todo 应用的类与职责，采用表格形式。`
- 代码审查：`以下是重构后的命令解析器，请指出可能的耦合点与建议的接口拆分方式。`

## 产出清单
- 新的 OOP 结构代码 + 测试。
- `day3-design.md` 文档与类图（可 ASCII）。
- Git 提交：`refactor: oop structure for todo cli`。

## 自查问题
1. 哪些类应该 `open`，哪些保持默认 `final`？
2. Data class 的 `copy` 用于哪些场景？
3. `sealed class Command` 与 `enum` 的区别？
4. 单例是否会影响测试可控性？如何解决？

## 复盘提示
- 记录 2 个“重构收益”与 1 个“隐患”。
- 列出下一步计划（如添加持久化、引入协程）。
