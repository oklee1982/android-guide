# 第1个月｜第1周｜第1天

## 今日使命
- 完成开发环境（Android Studio + Kotlin CLI）和 Git/GitHub 配置。
- 夯实 Kotlin 变量、类型系统与字符串模板；掌握 main 函数与命令行交互输入。
- 产出两个命令行小工具：温度转换器和基本计算器，具备输入校验与异常提示。

## 时长分配（8h）
1. 60 min：AI 讲义 + Kotlin 变量/类型快速测验。
2. 120 min：环境安装、Gradle/Kotlin CLI 验证、Git 仓库初始化。
3. 150 min：命令行程序实现与重构，覆盖参数输入/异常捕获。
4. 60 min：向 AI 提问“Basic syntax/Idioms”并整理讲义摘要（代替官方文档）。
5. 60 min：错题本与问题清单更新，输出概念卡片。
6. 30 min：面试速记（val vs var、String 模板、标准输入处理）。

## 知识块
- Kotlin 程序结构：`fun main(args: Array<String>)`、包声明、`println`。
- 变量与常量：`val`、`var`、类型推断、数字字面量、`BigDecimal` 初识。
- 字符串模板与多行字符串、原始字符串处理。
- 基本 IO：`readLine()`、命令行参数、异常安全输入。

## 讲义式要点

### 1. Kotlin 程序骨架与入口
```kotlin
package toolkit.intro

fun main(args: Array<String>) {
    println("Argument size = ${args.size}")
}
```
- `package` 可选，但建议与目录匹配，IDE 方便定位。
- `fun main()` 是 CLI 入口；`args: Array<String>` 可读取 `--mode=celsius` 这类参数。
- `println` 属于标准库，无需 `System.out.println`。

### 2. 变量、常量与类型推断
```kotlin
val city: String = "Beijing"    // 只读，必须初始化
var visits = 3                  // 类型推断为 Int，可重新赋值
val pi = 3.1415F                // 自动推断 Float
```
- `val` 表示引用不可变，`var` 可变；如果引用指向集合，集合内容仍可变。
- 推断遵循“能推断则省略类型”，但接口/公共 API 建议显式类型，方便阅读。
- 当数值精度重要时引入 `BigDecimal("0.1")`，避免浮点误差。

### 3. 字符串模板与原始字符串
```kotlin
val name = "Alice"
println("Hi, $name! Length=${name.length}")

val multiline = """
    |SELECT * FROM books
    |WHERE author = "$name"
""".trimMargin()
```
- `${}` 可嵌表达式，`$variable` 直接引用变量。
- `"""` 原始字符串会保留换行；配合 `trimMargin()` 清理缩进。

### 4. 基本 IO 与异常捕获
```kotlin
fun readInt(prompt: String): Int {
    print(prompt)
    return readLine()?.toIntOrNull()
        ?: error("请输入合法的整数")
}
```
- `readLine()` 返回 `String?`，为避免 `NullPointerException` 需要 `?.` 或 `?:`。
- CLI 程序以 `toIntOrNull()`、`toDoubleOrNull()` 做输入校验，失败时用自定义错误消息。

### 5. 示例：温度转换器骨架
```kotlin
enum class Mode { C2F, F2C }

fun convert(value: Double, mode: Mode): Double =
    when (mode) {
        Mode.C2F -> value * 9 / 5 + 32
        Mode.F2C -> (value - 32) * 5 / 9
    }

fun main(args: Array<String>) {
    val mode = args.firstOrNull()
        ?.uppercase()
        ?.let { Mode.valueOf(it) }
        ?: Mode.C2F
    val input = readLine()?.toDoubleOrNull()
        ?: return println("请输入数字")
    println("结果 = ${"%.2f".format(convert(input, mode))}")
}
```
- 通过 `enum class` 表达模式，`when` 语句直接返回数值。
- `firstOrNull()` + `let` 实现“存在才解析”，否则回退到默认模式。

### 6. 示例：计算器的错误处理
```kotlin
fun calculate(a: Double, b: Double, op: String): Double =
    when (op.lowercase()) {
        "add" -> a + b
        "sub" -> a - b
        "mul" -> a * b
        "div" -> require(b != 0.0) { "除数不能为0" }.let { a / b }
        else -> error("未知操作：$op")
    }
```
- `require` 触发 `IllegalArgumentException`，可给出用户友好提示。
- `error()` 用于非预期场景，快速失败，命令行可用 try-catch 包裹并打印信息。

## 实操任务
1. `env-check.md`：记录 IDE、JDK、Gradle、真机/模拟器配置步骤及截图。
2. `code/kotlin-week1/temp_converter.kt`：摄氏/华氏互转，支持 `--mode` 参数，非法输入给出提示。
3. `code/kotlin-week1/calculator.kt`：实现 `add/sub/mul/div`，包含 `try/catch`、除零与格式校验；添加 `--help`。
4. 在仓库根目录补充 `notes/day1-log.md`，包含“已知/未知/验证点”。

## AI Prompt 模板
- 讲义提示：`请按“概念要点→示例→常见坑”的结构讲解 Kotlin 变量/类型/字符串模板，并附 3 道选择题与答案理由。`
- 代码审查提示：`以下是我的 Kotlin 命令行代码片段，请列出输入校验、异常处理、命名的一条改进建议。`
- 复盘提示：`我今天在安装 Android Studio 时遇到的问题如下，请生成一份问题-原因-解决方案表格。`

## 产出清单
- 2 个可运行的 `.kt` 文件（含 README 运行说明）。
- VS Code/Android Studio 终端截图 + `env-check.md`。
- `notes/day1-log.md`：记录 3 个知识点、2 个疑惑、1 个下次验证项。

## 自查问题
1. 变量重新赋值后，是否影响其他引用？
2. `readLine()` 返回值为何需要空安全处理？
3. main 函数支持的入参形式有哪些？
4. CLI 工具对非法输入是否有统一出口？

## 复盘模板（可复制）
```
### 今日目标完成度
- 环境安装：✅ / ⚠️
- Kotlin 变量/类型：✅ / ⚠️
- 命令行 Demo：✅ / ⚠️

### 最大收获
1.
2.

### 最大卡点
- 现象：
- 排查：
- 结论：

### 明日计划
- 
```
