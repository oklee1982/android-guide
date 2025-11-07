# 第1个月｜第1周｜第5天

## 今日使命
- 深入 Kotlin 空安全体系：可空类型、`?.`、`?:`、`!!`、`let/apply/run/also/with`、`lateinit`、`typealias`、包结构与可见性修饰。
- 为图书管理 CLI 增加“用户偏好”模块：读取/保存 JSON 配置（最近搜索、收藏标签），利用扩展函数封装空值处理。
- 梳理可迁移到 Android 的空安全策略，形成“决策树”图。

## 时长分配（8h）
1. 60 min：AI 讲义 + 空安全测验。
2. 150 min：偏好模块设计与实现（配置文件读取、异常处理）。
3. 120 min：扩展函数/类型别名封装；撰写示例。
4. 60 min：向 AI 请教 `Null Safety`/`Scope Functions` 讲解并整理摘要。
5. 60 min：错题/决策树整理。
6. 30 min：面试速答（`let` vs `run`、`lateinit` 风险）。

## 知识块
- 空安全操作符组合、`if (value != null)` 与 `value?.let {}` 对比。
- `lateinit var` 与 `lazy` 属性委托。
- 作用域函数的区别与使用场景。
- 模块/包结构、`internal`、`private`、`public` 访问控制。
- `typealias` 与 DSL 友好性。

## 讲义式要点

### 1. 可空类型与操作符
```kotlin
val input: String? = readLine()
val length = input?.length ?: 0      // 空值 -> 0
val upper = input?.uppercase() ?: return println("为空")

val forced = input!!.length          // 极少使用，只在逻辑可证明非空时
```
- `?.`：若为 null 则直接返回 null，不执行后续。
- `?:`（Elvis）：可提供默认值、错误提示、`return`/`throw` 等。
- `!!`：只在 “此处绝不可能为空” 的情况下使用，失败会抛 `KotlinNullPointerException`。

### 2. Scope 函数对照
| 函数 | this/it | 返回值 | 场景 |
|------|---------|--------|------|
| `let` | `it` | lambda 结果 | 转换、链式调用、对可空值做处理 |
| `run` | `this` | lambda 结果 | 对同一对象做多步操作并返回表达式 |
| `apply` | `this` | 对象本身 | 初始化配置链 |
| `also` | `it` | 对象本身 | 插入日志/副作用 |
| `with(obj)` | `this` | lambda 结果 | 不能为 null 的多步骤操作 |

```kotlin
config?.let { println(it.name) } ?: println("未配置")
val prefs = UserPreferences().apply {
    theme = "dark"
    language = "zh-CN"
}
```

### 3. `lateinit`、`lazy`
```kotlin
lateinit var cache: Preferences

fun initCache() {
    cache = loadPreferences()
}

val token by lazy { readTokenFromDisk() }
```
- `lateinit` 只能用于 `var`、非基本类型；若未初始化直接访问会抛异常。
- `lazy` 适合只读属性，首次访问才初始化，可指定线程策略 `lazy(LazyThreadSafetyMode.NONE)`.

### 4. 包、可见性与 typealias
```kotlin
package config.preferences

internal class FilePreferenceStore

typealias JsonMap = Map<String, Any?>
```
- `internal` 限制在同一模块可见，`private` 限制在文件/类内部。
- `typealias` 为复杂泛型取别名，提升可读性，也便于将来替换实现。

### 5. 空安全扩展示例
```kotlin
fun String?.ifNullOrBlank(default: () -> String): String =
    if (isNullOrBlank()) default() else this!!

fun <T> T?.requireNotNull(message: String): T =
    this ?: error(message)
```
- 将常用的空值判断封装成扩展函数，避免重复写 if 判空。

### 6. 偏好模块读写流程
```kotlin
data class UserPreferences(
    val theme: String = "light",
    val favorites: List<String> = emptyList()
)

interface PreferenceStore {
    fun load(): UserPreferences?
    fun save(prefs: UserPreferences)
}

class JsonPreferenceStore(private val file: Path) : PreferenceStore {
    override fun load(): UserPreferences? =
        runCatching { Files.readString(file) }
            .mapCatching { deserialize(it) }
            .getOrNull()
    override fun save(prefs: UserPreferences) {
        Files.writeString(file, serialize(prefs))
    }
}
```
- 使用接口隔离存储实现，方便将来替换数据库或 DataStore。
- 读取失败时返回默认值或提示用户重新初始化。

## 实操任务
1. `config/UserPreferences.kt`：定义偏好数据类、序列化/反序列化函数（可用 `kotlinx.serialization` 或简单 JSON 手写）。
2. 在 CLI 中加入命令 `prefs show/set --key=xxx --value=yyy`，调用扩展函数处理空值。
3. `notes/day5-null-safety.md`：绘制空安全决策树（ASCII/Markdown 图表）。
4. `extensions/NullSafetyExtensions.kt`：封装常用空值处理（例如 `String?.ifNullOrBlank(default: () -> String)`）。

## AI Prompt 模板
- 讲义：`请输出 Kotlin 空安全操作符对照表，并给出 5 道情景题和答案解释。`
- 代码生成：`请根据以下 JSON 结构生成 Kotlin data class + 序列化/反序列化示例。`
- 复盘：`以下是我总结的空安全决策树，请帮我审查是否遗漏常见分支。`

## 产出清单
- 偏好模块代码 + 运行示例。
- `NullSafetyExtensions.kt` 与使用示例。
- `day5-null-safety.md` 决策树与 QA。
- 日志 `notes/day5-log.md`。

## 自查问题
1. 何时可以使用 `!!`？是否全部替换为 `?:` 更安全？
2. `lateinit` 在何种类型上不可用？
3. Scope 函数之间的返回值和 `this/it` 绑定差异？
4. JSON 读写失败时的回退策略是什么？

## 复盘提示
- 记录 2 个“空安全 Bug”案例与修复要点。
- 写下 3 个准备迁移到 Android ViewBinding/Repository 层的空安全策略。
