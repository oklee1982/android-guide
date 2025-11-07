# 第1个月｜第1周｜第2天

## 今日使命
- 掌握 Kotlin 条件/循环/表达式语义及函数声明、默认与命名参数、`when` 的高级用法。
- 完成命令行版「待办事项」程序，覆盖 CRUD、过滤、排序逻辑。
- 建立测试用例枚举表，开始引入单元测试（如 `kotest` 或 `JUnit` CLI）。

## 时长分配（8h）
1. 70 min：AI 讲义 + 控制流小测。
2. 160 min：设计与实现待办 CLI（需求拆解→编码→交互优化）。
3. 90 min：为关键函数写 5+ 条测试/假数据验证脚本。
4. 60 min：向 AI 提问“控制流/函数进阶”并整理讲义摘要（代替官方文档）。
5. 60 min：错题本/FAQ 更新；记录 when 使用误区。
6. 40 min：面试题速答与讲解（`when` 作为表达式、尾递归）。

## 知识块
- if/when 作为表达式，`when` 与区间、类型匹配结合。
- 循环：`for-in`、`while`、`repeat`、标签跳转（`break@loop`）。
- 函数：默认值、命名参数、`vararg`、单表达式函数、尾递归示例。
- 简易测试：`kotlin.test` 断言、输入模拟。

## 讲义式要点

### 1. if/when 作为表达式
```kotlin
fun grade(score: Int): String =
    if (score >= 90) "A"
    else if (score >= 80) "B"
    else "C"

fun describeInput(token: Any?) = when (token) {
    null -> "empty"
    in 0..9 -> "single digit"
    is String -> "text(${token.length})"
    else -> "unknown"
}
```
- `if`/`when` 返回值可直接赋给变量或作为函数结果；最后一行就是表达式值。
- `when` 支持区间、类型匹配、任意条件；确保覆盖所有情况，必要时用 `else`.

### 2. 循环与标签
```kotlin
outer@ for (row in 0..2) {
    for (col in 0..2) {
        if (row == col) continue@outer
        println("row=$row, col=$col")
    }
}
repeat(3) { println("repeat #$it") }
```
- `break@label` / `continue@label` 控制多层循环。
- `repeat(n)` 更语义化的固定次数循环。

### 3. 函数技巧
```kotlin
fun formatTodo(
    title: String,
    completed: Boolean = false,
    tags: List<String> = emptyList()
) = "$title | done=$completed | tags=${tags.joinToString()}"

fun sumAll(vararg numbers: Int) =
    numbers.fold(0) { acc, n -> acc + n }

tailrec fun factorial(n: Int, acc: Long = 1): Long =
    if (n <= 1) acc else factorial(n - 1, acc * n)
```
- 命名参数提升可读性：`formatTodo(title = "阅读", completed = true)`.
- `vararg` 接受任意数量参数；传数组需 `*array`.
- `tailrec` 在满足尾递归条件时由编译器转为循环。

### 4. 命令解析与 sealed class
```kotlin
sealed class Command {
    data class Add(val title: String) : Command()
    data class Complete(val id: Int) : Command()
    data class List(val filter: String?) : Command()
    object Help : Command()
}

fun parse(input: List<String>): Command = when (input.firstOrNull()) {
    "add" -> Command.Add(input.getOrNull(1) ?: error("缺少标题"))
    "done" -> Command.Complete(input.getOrNull(1)?.toIntOrNull()
        ?: error("缺少ID"))
    "list" -> Command.List(input.getOrNull(1))
    else -> Command.Help
}
```
- `sealed class` 限定子类范围，`when(command)` 可穷尽所有分支，不需要 `else`.
- `error()`/`require()` 搭配 try-catch，可以统一错误提示。

### 5. CLI 框架搭建示例
```kotlin
fun main() {
    val repo = TodoRepository()
    while (true) {
        print("> ")
        val command = readLine()?.split(" ") ?: break
        try {
            when (val result = handleCommand(parse(command), repo)) {
                is Output.Text -> println(result.message)
                Output.Exit -> return
            }
        } catch (e: IllegalArgumentException) {
            println("输入错误：${e.message}")
        }
    }
}
```
- 将 `parse`、`handleCommand` 拆开，方便测试。
- 所有输入都经统一入口处理，便于记录日志或扩展历史功能。

## 实操任务
1. `code/kotlin-week1/todo_cli.kt`：
   - 支持新增/删除/完成/清单、按照优先级或截止日期排序。
   - 使用 `when` 解析指令；用 `sealed class Command` 表达命令类型。
2. `notes/todo-spec.md`：列出数据模型（`data class Todo`）与命令设计。
3. `tests/TodoTests.kt`：至少覆盖 5 条逻辑，演示 `assertEquals`、`assertFails`。
4. 记录 CLI 交互示例与输出截图。

## AI Prompt 模板
- 知识点：`请输出 Kotlin if/when/循环的脑图式要点，并给出 3 个“纠正常见误解”的练习题。`
- 代码生成辅助：`根据以下 JSON 结构，为待办 CLI 生成解析示例和 when 语句骨架。`
- 测试辅助：`我写了以下函数，请帮我列出 5 条有效的测试用例（输入+期望+关注点）。`

## 产出清单
- `todo_cli.kt` 完整代码 + README 运行方式。
- `todo-spec.md` 设计文档（包含命令对照表）。
- 测试文件或脚本输出截图。
- 日志 `notes/day2-log.md`（延续 day1 模板）。

## 自查问题
1. `when` 是否覆盖所有分支？`else` 是否必须？
2. 循环中若提前返回是否会影响资源释放？
3. 函数的默认参数在 Java 调用时如何表现？
4. CLI 输入解析是否具备统一的错误提示？

## 复盘 Checklist
- [ ] 代码通过 `kotlinc` 编译并运行 3 次场景测试。
- [ ] 复述 AI 讲义中的 5 条要点并写入日志。
- [ ] 错题本新增控制流章节。
- [ ] 提交 Git commits：`feat: todo cli v1`、`test: add todo tests`。
