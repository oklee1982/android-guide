# Kotlin 第一周学习梳理

# Kotlin第一周学习知识体系梳理（按优先级与学习逻辑）

## 一、核心基础（前置必备，高频使用）

### 1. 语法基础

- **变量与常量**（Day1）
  
  - `val`（只读，优先使用）与`var`（可变）的区别及使用场景
  
  - 类型推断与显式声明（`val name: String = "Alice"`）
  
  - ~~高精度数值处理（`BigDecimal`用于货币等场景）~~

- **函数定义**（Day1）
  
  - 基本语法与参数传递
  
  - **默认参数+命名参数**（核心特性，简化调用）
    
    ```Kotlin
    fun greet(name: String, prefix: String = "Hello")
    greet("Bob", prefix = "Hi") // 命名参数调用
    ```
  
  - 可空返回值（`Int?`表示可能返回null）

- **控制流**（Day2）
  
  - `if`作为表达式（替代三元运算符）
  
  - `when`表达式（强大的多分支处理，支持类型判断、区间匹配）
  
  - 循环与区间（`for (i in 0 until 10)`、`forEach`高阶函数）

### 2. 集合操作（高频业务场景）

- **基础集合**（Day4）
  
  - 不可变集合（`listOf()`）与可变集合（`toMutableList()`）的设计原则
  
  - 常用高阶函数（必会）：
    
    - `filter`（筛选）、`map`（转换）
    
    - `find`（查找）、`sortedBy`/`sortedByDescending`（排序）
    
    - `all`/`any`（条件判断）、`sum`（求和）

- ~~**序列（Sequence）**（Day4）~~
  
  - 延迟计算特性（适合大列表>1w元素）
  
  - 终止操作触发执行（`toList()`、`eachCount()`）

### 3. 空安全（Kotlin核心特性）

- **基础机制**（Day5）
  
  - 可空类型（`String?`）与非空类型（`String`）的区分
  
  - 安全操作符：`?.`（空安全调用）、`?:`（空默认值）
  
  - 作用域函数（高频使用）：
    
    <table class="ace-table"><colgroup><col width="200"><col width="200"><col width="200"><col width="200"></colgroup><tbody><tr><th><div class="ace-line ace-line old-record-id-Up41fMOotdkaEccclrDam0nc66K">函数</div></th><th><div class="ace-line ace-line old-record-id-XhE8fu1dXdZv1oc8j2K7mr4fqPE">内部指代</div></th><th><div class="ace-line ace-line old-record-id-QbR5f3q2RdTQM7cSBAMig08ehjU">返回值</div></th><th><div class="ace-line ace-line old-record-id-KcfGfv5UhdopFjcsHYxyIIYcHcn">核心场景</div></th></tr><tr><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-LO9Gf4qgBdM253c46UbfIVPeW8T"><code>let</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-Y7WkfuQAudXqOtclU5pRLwvbGWa"><code>it</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-C9XBfiNsOddHVEcNdxT5EcaeP1F">lambda结果</div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-ERdCfWxytddlvycbp2Cl6dqrwFZ">可空对象处理、数据转换</div></td></tr><tr><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-EIijfLxpJdpbxjccNdoHenDzy47"><code>apply</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-UJSGfyEhFdKlDMcMGKbQ0mxgbJy"><code>this</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-LrzZfNQCydhQMfcp7d7vpIHdMvm">对象本身</div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-LLcvfSitedWo3HcTQ6uestjcoOb">初始化配置</div></td></tr><tr><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-Mck0fjPcddysU1cqwolVX3k1jXV"><code>run</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-Fyhnffxzpdkzg8cha0TKq60f7MZ"><code>this</code></div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-S794fMiY2dlOZqcDlGszEQxgZNw">lambda结果</div></td><td rowspan="1" colspan="1"><div class="ace-line ace-line old-record-id-DcuNfQZU8dT2L8cS4YUFLuFf8TD">同一对象多步操作</div></td></tr></tbody></table>

- **延迟初始化**（Day5）
  
  - `lazy`（懒加载只读属性，首次访问初始化）
  
  - `lateinit`（非空属性延迟初始化，注意未初始化风险）

## 二、面向对象编程（进阶核心）

### 1. 核心组件

- **数据类（data class）**（Day3）
  
  - ~~自动生成`equals`、`hashCode`、`toString`、`copy`方法~~
    
    ```Kotlin
    data class TodoItem(val id: Int, val title: String)
    ```

- **接口（Interface）**（Day3）
  
  - 定义行为契约，支持默认实现
  
  - 类可实现多个接口（解决多继承问题）

### 2. 设计原则（代码架构基础）

- **单一职责原则（SRP）**：一个类只负责一件事（如拆分解析、存储、业务逻辑）

- **~~依赖倒置原则（DIP）**：依赖抽象而非具体实现（如`TodoService`依赖`TodoRepository`接口）~~

- **~~开闭原则（OCP）**：对扩展开放，对修改关闭（通过接口扩展新功能）~~

## 三、协程（异步编程核心）

### 1. 基础概念

- 本质：~~轻量级线程~~，通过挂起/恢复实现异步逻辑同步化（Day6）

- `suspend`函数：标记可挂起的函数，只能在协程或其他`suspend`函数中调用

### 2. ~~结构化并发（核心机制）~~

- 作用：通过`CoroutineScope`管理生命周期，避免协程泄漏

- 父协程等待所有子协程完成，取消操作可传递（Day6）

### 3. ~~并行与串行（实操重点）~~

```Kotlin
// 串行：总耗时=任务1+任务2
suspend fun serialFetch() { fetchA(); fetchB() }

// 并行：总耗时≈最长任务时间
suspend fun parallelFetch() = coroutineScope {
  awaitAll(async { fetchA() }, async { fetchB() })
}
```

## 四、实践与工程化（应用层）

### 1. 命令行程序设计

- 数据模型设计（`data class`存储核心信息）

- <u>命令解析（`when`表达式处理`Command`密封类）</u>

- ~~业务分层（解析、存储、服务分离，Day2、Day3）~~

### 2. ~~测试基础~~

- 单元测试框架（`@Test`、`assertEquals`，Day2、Day6）

- 协程测试（`runTest`测试挂起函数，Day6）

## 五、了解内容（扩展认知）

1. **可见性修饰符**（Day5）：`public`（默认）、`private`、`internal`、`protected`的访问范围

2. **`typealias`**（Day5）：给复杂类型起别名（如`typealias JsonMap = Map<String, Any?>`）

3. **~~尾递归优化**（Day2）：`tailrec`修饰符的使用条件（最后一步必须是自身调用）~~

4. **协程调度器**（Day6）：`Dispatchers.IO`（IO密集型）、`Dispatchers.Default`（CPU密集型）的区别

## 学习路径建议

1. 先掌握**语法基础**（变量、函数、控制流）和**集合操作**（日常开发高频使用）

2. 理解**空安全**机制（Kotlin特色，避免空指针异常的核心）

3. 学习**OOP设计原则**（写出可维护代码的关键）

4. 最后掌握**协程**（异步编程的现代解决方案）
