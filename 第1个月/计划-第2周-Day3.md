# 第1个月｜第2周｜第3天

## 今日使命
- 熟悉常用 View 组件（TextView/Button/ImageView/EditText/Material 组件）与布局容器（LinearLayout、ConstraintLayout）技巧。
- 设计“课程信息卡”与表单页面，完成资源抽取（colors/dimens/styles）。
- 输出 UI 组件速查表和布局截图，形成 `notes/week2/ui-basics.md`、`view-cheatsheet.md`。

## 时长分配（8h）
1. 70 min：AI 讲义（常用 View + ConstraintLayout）与示例分析。
2. 150 min：实现课程信息卡布局（含 ConstraintLayout 练习）。
3. 120 min：实现 `FormActivity`（TextInputLayout + EditText + Button），加入输入校验。
4. 80 min：整理 `ui-basics.md`、`view-cheatsheet.md`、截图。
5. 60 min：资源抽取、样式复用、夜间模式适配。
6. 40 min：复盘与问题清单。

## 知识块
- View 常用属性：`textAppearance`、`drawableStart`、`maxLines`、`contentDescription`。
- LinearLayout：水平/垂直布局、`layout_weight`、`gravity`、嵌套与性能注意事项。
- ConstraintLayout：Constraint、Guideline、Barrier、Chain、`ConstraintSet`。
- Material 组件：`MaterialCardView`、`TextInputLayout`、`MaterialButton`。
- 输入校验：`TextWatcher`、`doAfterTextChanged`、错误提示策略。
- 资源规范：`colors.xml`、`dimens.xml`、`styles.xml`、`theme overlay`。

## 讲义式要点
1. **TextView/Button/ImageView 基础**
   ```xml
   <TextView
       android:id="@+id/tvTitle"
       style="@style/Text.Title"
       android:text="课程标题"
       android:maxLines="2"
       android:ellipsize="end" />
   ```
   - 样式统一放在 `styles.xml`；多语言使用 `@string/course_title`.
2. **LinearLayout + ConstraintLayout 约束模式**
   ```xml
   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:gravity="center_vertical"
       android:orientation="horizontal">
       <TextView
           android:layout_width="0dp"
           android:layout_height="wrap_content"
           android:layout_weight="1"
           android:text="设置项" />
       <ImageView
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:src="@drawable/ic_chevron" />
   </LinearLayout>
   ```
   - LinearLayout 适合快速堆叠/横向排列，`layout_weight` 控制占比，注意避免深度嵌套。
   ```xml
   app:layout_constraintStart_toStartOf="parent"
   app:layout_constraintEnd_toEndOf="parent"
   app:layout_constraintTop_toTopOf="parent"
   app:layout_constraintWidth_default="spread"
   ```
   - ConstraintLayout 支持链式约束，`layout_constraintHorizontal_bias` 控制偏移。
3. **MaterialCardView & ImageView**
   ```xml
   <com.google.android.material.card.MaterialCardView
       app:cardCornerRadius="@dimen/card_radius"
       app:strokeColor="@color/card_border">
       <ImageView android:scaleType="centerCrop" />
   </com.google.android.material.card.MaterialCardView>
   ```
4. **TextInputLayout + 输入校验**
   ```kotlin
   binding.inputLayout.editText?.doAfterTextChanged {
       binding.inputLayout.error = if (it.isNullOrBlank()) "必填" else null
   }
   ```
5. **资源抽取**
   - `values/dimens.xml`：间距、字号；`values/colors.xml`：品牌色；`values-night/colors.xml`：夜间模式。
   - 通过 `themeOverlay` 提取按钮样式，减少重复属性。

## 实操任务
1. `layout/item_course.xml`：包含图片、标题、副标题、描述、按钮；使用 ConstraintLayout + MaterialCardView。
2. `layout/item_setting.xml`：使用 LinearLayout（横向 + `layout_weight`）、TextView/Icon 组合，展示设置项；练习常用属性与对齐方式。
3. `FormActivity`：使用 ViewBinding（可等 Day4 统一）或 `findViewById`；实现输入校验、提交按钮、Toast/ Snackbar 提示。
4. 资源抽取：创建 `dimens.xml`、`colors.xml`、`styles.xml`，并在布局中引用；增加 `values-night`.
5. `notes/week2/ui-basics.md`：记录常用 View 属性、LinearLayout/ConstraintLayout 技巧、输入校验方案。
6. `notes/week2/view-cheatsheet.md`：列表化 10+ 个常用属性/示例/注意事项；附两张布局截图（保存到 `assets/screenshots/day3-*.png`）。

## AI Prompt 模板
1. `请用表格对比 TextView/Button/ImageView/EditText 的常见属性、使用场景、可复用样式。`
2. `以下是我画的布局草图，请生成 ConstraintLayout XML 并指出可抽取的资源与样式。`
3. `请给出 TextInputLayout + EditText 表单校验的示例，要求包含错误提示与焦点管理。`

## 产出清单
- `layout/item_course.xml`、`layout/activity_form.xml`。
- `notes/week2/ui-basics.md`、`notes/week2/view-cheatsheet.md`。
- 资源文件更新：`values/colors.xml`、`dimens.xml`、`styles.xml`、`values-night`.
- `assets/screenshots/day3-card.png`、`day3-form.png`。
- 日志 `notes/week2/day3-log.md`。

## 自查问题
1. 布局是否使用 ConstraintLayout，约束是否正确（无歧义/无警告）？
2. 是否对 TextView/ImageView 设置 `contentDescription`、`importantForAccessibility`？
3. 输入校验逻辑是否可复用，错误提示是否友好？
4. 资源是否统一抽取（颜色/尺寸/样式），夜间模式是否生效？
5. 文档与截图是否及时更新？

## 复盘模板
```
### 今日完成度
- 课程卡片布局：✅ / ⚠️
- FormActivity：✅ / ⚠️
- 文档/截图：✅ / ⚠️

### 收获
1.
2.

### 问题与排查
- 现象：
- 排查：
- 结论：

### 明日准备
- ViewBinding/RecyclerView 需要关注的点：
- 需要 AI 协助的问题：
```
