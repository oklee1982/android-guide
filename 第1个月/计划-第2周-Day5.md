# 第1个月｜第2周｜第5天

## 今日使命
- 理解 Fragment 生命周期、BackStack 管理、`FragmentManager` 操作。
- 在 Demo 中实现 `DashboardFragment`、`DetailFragment`，支持按钮导航、BackStack、状态展示。
- 绘制导航关系图、记录常见问题，必要时体验 Navigation Component（进阶，选做）。

## 时长分配（8h）
1. 70 min：AI 讲义（Fragment 生命周期、导航模式）与问答。
2. 150 min：实现 Fragment 布局与切换（`supportFragmentManager.commit`）。
3. 150 min：实现 BackStack、参数传递、状态恢复；输出导航图。
4. 80 min：撰写 `notes/week2/navigation.md`、`notes/week2/fragment-notes.md`。
5. 40 min：探索 Navigation Component（进阶：可选）或记录未来引入计划。
6. 30 min：复盘与问题清单。

## 知识块
- Fragment 生命周期：`onAttach`→`onCreate`→`onCreateView`→`onViewCreated`→`onStart`→`onResume`，与 `onDestroyView`/`onDestroy`.
- FragmentManager 操作：`commit { replace/add }`、`addToBackStack`、`setReorderingAllowed`.
- 参数传递：`FragmentArgs`、`arguments`、`bundleOf`；`requireArguments`.
- BackStack 行为：`popBackStack`、`popBackStackImmediate`、`isStateSaved`.
- （进阶）Navigation Component：NavHostFragment、NavGraph、SafeArgs。

## 讲义式要点
1. **Fragment 生命周期对比**
   - `onCreateView` 创建视图，`onDestroyView` 清理 View；与 Activity 生命周期交错。
   - 适合在 `onViewCreated` 设置监听、ViewBinding；`onDestroyView` 释放 `_binding`.
2. **Fragment 切换**
   ```kotlin
   supportFragmentManager.commit {
       setReorderingAllowed(true)
       replace(R.id.fragmentContainer, DetailFragment.newInstance(courseId))
       addToBackStack("detail")
   }
   ```
3. **参数传递**
   ```kotlin
   companion object {
       fun newInstance(id: String) = DetailFragment().apply {
           arguments = bundleOf(ARG_ID to id)
       }
   }
   ```
4. **BackStack 观察**
   - 使用 `supportFragmentManager.addOnBackStackChangedListener` 记录堆栈变化。
   - `onBackPressedDispatcher` 控制返回行为。
5. **导航图绘制**
   - 将 `MainActivity`、`DashboardFragment`、`DetailFragment`、`SettingsActivity` 等节点画成图，标注 Action/参数/BackStack。
6. **Navigation Component（进阶）**
   - 使用 `NavHostFragment` + `nav_graph.xml`，处理 SafeArgs；非必要可先了解概念。

## 实操任务
1. `DashboardFragment`：展示课程列表入口、统计信息；使用 ViewBinding。
2. `DetailFragment`：接收课程 ID，展示详情；提供“编辑/返回”按钮。
3. `MainActivity`：嵌入 `FragmentContainerView`，提供按钮在 Fragment 间切换；实现 BackStack 日志。
4. `notes/week2/navigation.md`：包含导航图、BackStack 测试步骤、参数说明。
5. `notes/week2/fragment-notes.md`：记录生命周期差异、`childFragmentManager`、`viewLifecycleOwner`、常见坑。
6. （进阶，选做）创建 `nav_graph.xml`，体验 Navigation Component；记录在文档中。

## AI Prompt 模板
1. `请生成 Fragment 生命周期 vs Activity 生命周期的对照表，并解释 viewLifecycleOwner 的意义。`
2. `以下是我的 Fragment 切换需求，请给出 commit/replace/add/backStack 的建议，并指出注意事项。`
3. （进阶）`请讲解 Navigation Component 的基本概念，并生成包含两个 Fragment 的最小示例。`

## 产出清单
- Fragment 布局、切换代码、BackStack 日志。
- `notes/week2/navigation.md`、`notes/week2/fragment-notes.md`。
- 导航图截图 `assets/screenshots/day5-nav.png`。
- `notes/week2/day5-log.md`。

## 自查问题
1. Fragment 是否正确处理 ViewBinding 生命周期（`_binding = null`）？
2. BackStack 是否按预期工作（按返回键回到 Dashboard、Detail）？
3. Fragment 参数是否集中定义常量，并做空值校验？
4. 是否记录 Fragment/Activity 生命周期差异与常见坑？
5. 是否计划/记录 Navigation Component（进阶）后续引入策略？

## 复盘模板
```
### 今日完成度
- Fragment 导航：✅ / ⚠️
- 文档/导航图：✅ / ⚠️
- 进阶探索（Navigation Component）：完成 / 了解 / 未进行

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查：
- 结论：

### 明日准备
- 状态管理/配置变更关注点：
- 需要 AI 协助的问题：
```
