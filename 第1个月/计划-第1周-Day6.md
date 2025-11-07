# 第1个月｜第1周｜第6天

## 今日使命
- 进入协程世界：理解协程上下文、`suspend` 函数、`launch`/`async`、`withContext`、异常处理与结构化并发。
- 为图书管理 CLI 添加“并行数据源”示例：模拟网络请求（`delay`），与本地缓存合并；输出日志对比串行 vs 并行耗时。
- 编写最小单元测试验证数据合并逻辑，并生成周总结。

## 时长分配（8h）
1. 80 min：协程核心概念讲义 + 表格记忆。
2. 150 min：实现网络模拟模块，整合到 CLI。
3. 120 min：补充单元测试、异常场景（如网络失败）及日志。
4. 60 min：向 AI 提问“Coroutines 基础/结构化并发”并整理摘要（代替官方指南）。
5. 80 min：周总结撰写、错题归档、README 更新。
6. 30 min：面试 Q&A（`suspend` vs `Thread.sleep`、`CoroutineScope` 管理）。

## 知识块
- 协程构建器：`runBlocking`、`launch`、`async-await`。
- `Dispatchers.Default/IO/Main`（Main 简述即可），`SupervisorJob`。
- 异常传播与 `CoroutineExceptionHandler`。
- 结构化并发：父子作用域、`coroutineScope {}`、取消传播。

## 讲义式要点

### 1. 协程构建器速览
```kotlin
fun main() = runBlocking {
    launch { delay(100); println("launch in ${Thread.currentThread().name}") }
    val result = async { fetchRemote() }
    println("result = ${result.await()}")
}
```
- `runBlocking` 只在 CLI 或测试入口使用，用于桥接阻塞代码。
- `launch` 返回 `Job`，适合 fire-and-forget 任务；`async` 返回 `Deferred<T>`，需要 `await`.

### 2. `suspend` 函数与 `withContext`
```kotlin
suspend fun loadBooks(): List<Book> =
    withContext(Dispatchers.IO) {
        delay(200) // 模拟网络
        loadFromDisk()
    }
```
- `suspend` 表示函数内部可以挂起；只能被其他 `suspend` 或协程调用。
- `withContext` 切换到指定调度器（IO、Default），并返回闭包结果。

### 3. 结构化并发与取消
```kotlin
suspend fun fetchTwoSources(): List<Book> = coroutineScope {
    val hot = async { remoteHot() }
    val recommend = async { remoteRecommend() }
    (hot.await() + recommend.await()).distinctBy { it.id }
}
```
- `coroutineScope {}` 保证子协程完成/取消后才返回。
- 取消：`job.cancel()` 或 `scope.cancel("reason")`，需要在 `delay`/挂起点才能响应；IO 操作可在 finally 释放资源。

### 4. 错误处理
```kotlin
val handler = CoroutineExceptionHandler { _, throwable ->
    println("捕获异常：${throwable.message}")
}

fun main() = runBlocking {
    val job = launch(handler + SupervisorJob()) {
        launch { error("子任务崩溃") }
        launch { delay(1000); println("仍然执行") }
    }
    job.join()
}
```
- 普通 `Job` 中某个子协程崩溃会取消整个作用域；`SupervisorJob` 允许兄弟协程互不影响。
- 在业务代码中捕获异常并转换为用户可见的提示，同时写入日志。

### 5. 并行 vs 串行耗时对比
```kotlin
suspend fun serialFetch() {
    val start = now()
    remoteHot()
    remoteRecommend()
    println("serial took ${now() - start}ms")
}

suspend fun parallelFetch() = coroutineScope {
    val start = now()
    awaitAll(async { remoteHot() }, async { remoteRecommend() })
    println("parallel took ${now() - start}ms")
}
```
- 使用 `awaitAll` 等方式并行执行独立任务，记录日志对比。
- 当接口存在依赖关系时仍需串行，避免无意义并行。

### 6. 测试协程代码
```kotlin
class SyncTests {
    @Test
    fun `combine remote with cache`() = runTest {
        val repo = FakeRepo(...)
        val data = syncUseCase(repo).invoke()
        assertEquals(expectedSize, data.size)
    }
}
```
- `kotlinx-coroutines-test` 提供 `runTest`、`TestDispatcher` 模拟时间。
- CLI 项目也可用 `runBlockingTest` 断言协程行为，避免真实 `delay`.

## 实操任务
1. `code/kotlin-week1/network/RemoteSource.kt`：模拟两个数据源（热门图书/推荐），使用 `delay`。
2. 在 CLI 中新增命令 `sync remote`：并行获取两个列表并合并去重，输出耗时对比。
3. `tests/SyncTests.kt`：验证合并逻辑、错误回退（本地缓存 fallback）。
4. `notes/week1-summary.md`：
   - 本周完成内容、收获、待解决问题。
   - 下周（Android Studio + Activity）预研清单。

## AI Prompt 模板
- 讲义：`请用表格说明 Kotlin 协程的核心概念，并附并行抓取两个接口的示例（含错误处理）。`
- 调试：`以下协程代码运行结果异常，请分析可能的作用域或 Dispatcher 问题并给出修改建议。`
- 周总结：`根据以下 bullet，帮我生成一份结构化的学习周报（完成情况/问题/计划）。`

## 产出清单
- 协程示例代码 + CLI 命令运行截图。
- 单元测试或日志，展示并行 vs 串行耗时比较。
- `week1-summary.md`（可直接发布）。

## 自查问题
1. `runBlocking` 适合放在生产代码的哪里？
2. 何时需要 `withContext(Dispatchers.IO)`？
3. 协程取消如何确保资源释放？
4. 合并逻辑是否考虑重复项、排序、错误兜底？

## 复盘提示
- 列出 2 个“协程使用场景”与 1 个“不适合协程”的例子。
- 写下对 Android 中 ViewModelScope/生命周期的疑问，为下周做准备。
