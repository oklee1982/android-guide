# 第1个月｜第1周｜第4天

## 今日使命
- 熟悉 Kotlin 集合（不可变/可变 List/Set/Map）、序列、常见高阶函数、泛型与内联函数的基本概念。
- 构建“图书管理 CLI”：从 JSON/CSV 读取数据，支持搜索/筛选/排序，自定义扩展函数封装集合操作。
- 探索性能差异：列表大小 >1w 时 `map` vs `sequence` 的差异，记录实验数据。

## 时长分配（8h）
1. 70 min：集合与泛型讲义 + 小测。
2. 180 min：CLI 需求分析、数据源准备、功能编码。
3. 90 min：性能对比试验，记录日志与结论。
4. 60 min：向 AI 追问“Collections/Generics/Sequences”要点并整理讲义（代替官方文档）。
5. 60 min：错题本 + Cheat Sheet 整理。
6. 40 min：面试速答（`Sequence`、`inline`、`reified` 基本概念）。

## 知识块
- 集合 API：`map/filter/flatMap/groupBy/associate`、`sortedBy`。
- 可变 vs 不可变集合、`copy` 与防御性编程。
- `sequence` 延迟计算、`generateSequence`。
- 泛型：上界 `where T : Comparable<T>`，星投影、`reified` 限定。
- `inline`/`noinline`/`crossinline` 用途。

## 讲义式要点

### 1. 不可变 vs 可变集合
```kotlin
val authors = listOf("Alice", "Bob")        // 不可变引用 + 内容
val mutableAuthors = authors.toMutableList()
mutableAuthors += "Carol"

fun printNames(names: List<String>) {}      // 传入方无法修改
fun addName(names: MutableList<String>) {}  // 明确可修改
```
- 接口设计优先使用不可变 `List`，防止调用方误改。
- 若需要返回快照，使用 `toList()` 复制，避免外部持有内部引用。

### 2. 常用高阶函数组合
```kotlin
data class Book(val title: String, val tags: List<String>, val rating: Double)

val sciFiTitles = books
    .filter { "Sci-Fi" in it.tags }
    .sortedByDescending { it.rating }
    .map { it.title }
    .take(5)
```
- `map`/`filter`/`sortedBy` 可串联；使用 `take`、`drop` 控制结果。
- 需要键值映射时用 `associateBy { it.id }`，统计分类用 `groupBy`.

### 3. Sequence 延迟计算
```kotlin
val topTags = books
    .asSequence()
    .flatMap { it.tags.asSequence() }
    .groupingBy { it }
    .eachCount()
    .entries
    .sortedByDescending { it.value }
    .take(10)
```
- `.asSequence()` 让中间步骤惰性执行，适合大列表或昂贵计算。
- 别忘记最终需要“终止操作”如 `toList()`、`eachCount()`，否则不会执行。

### 4. 泛型约束与 `where`
```kotlin
fun <T : Comparable<T>> maxOfTwo(a: T, b: T): T =
    if (a >= b) a else b

fun <T> T.requireNotNull(field: String): T where T : Any? =
    this ?: error("$field 不可为空")
```
- `<T : Comparable<T>>` 限定类型，函数内部才能调用 `compareTo`.
- 多个约束可以 `where T : CharSequence, T : Appendable`.

### 5. reified + inline
```kotlin
inline fun <reified T> List<Any>.filterType(): List<T> =
    filterIsInstance<T>()

inline fun <T> measure(tag: String, block: () -> T): T {
    val start = System.currentTimeMillis()
    return block().also {
        println("$tag 耗时 ${System.currentTimeMillis() - start}ms")
    }
}
```
- `reified` 只能用在 `inline` 泛型函数中，允许直接引用 `T::class`.
- `inline` 可以减少高频小函数的开销，但包含 `return`、`try` 时需注意 `crossinline/noinline`.

### 6. JSON/CSV 读写与异常处理
```kotlin
fun loadBooks(path: Path): List<Book> =
    runCatching { Files.readAllLines(path) }
        .mapCatching { lines -> lines.drop(1).map(::parseCsvLine) }
        .getOrElse { throwable ->
            println("读取失败：${throwable.message}")
            emptyList()
        }
```
- 所有 IO 都包裹在 `runCatching` 或 `try-catch` 中，错误统一汇报给 CLI。
- 解析失败时写入错误日志，避免静默丢数据。

## 实操任务
1. `data/books.json` 或 `.csv` 准备 50+ 条样例数据。
2. `code/kotlin-week1/library_cli.kt`：
   - 命令：`list`、`search <keyword>`、`filter --tag=xxx`、`sort --field=rating`。
   - 采用扩展函数如 `List<Book>.search(keyword: String)`。
3. `notes/day4-performance.md`：记录 Sequence 实验（数据量、执行时间、内存观察）。
4. `CheatSheet-Collections.md`：整理常用函数、示例、常见坑。

## AI Prompt 模板
- 数据生成：`请生成 30 条 JSON 图书数据，字段含 title/author/tag/rating/year，以列表形式返回。`
- 优化建议：`下面是我的集合操作代码，请指出可用 Sequence 或 inline 函数优化的 3 个点。`
- 复盘：`请根据以下实验数据，帮我总结 Sequence vs List 的优缺点表格。`

## 产出清单
- CLI 源码 + 运行截图。
- 数据文件 `books.json/csv`。
- 性能笔记与 Cheat Sheet。
- 日志 `notes/day4-log.md`。

## 自查问题
1. 扩展函数是否滥用？是否应该放在 utilities 包？
2. Sequence 何时需要 `toList()` 终止？
3. 泛型上界如何限制某函数只接受可比较类型？
4. CSV/JSON 解析中的异常是否捕获？

## 复盘提示
- 记录 1 次“性能优化前后”对比。
- 输出 3 个可迁移到 Android RecyclerView 的想法。
