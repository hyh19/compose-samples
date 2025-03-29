# 项目中有哪些自定义 View/ViewGroup？请提供每个自定义 View 的相对路径和核心方法位置

项目中的自定义视图组件主要是基于 Jetpack Compose 实现的。这是一个使用 Compose 框架构建的应用程序，而不是传统的 View/ViewGroup 架构。以下是主要的自定义组件：

1. **BaselineHeightModifier**
   - 相对路径：`app/src/main/java/com/example/compose/jetchat/components/BaseLineHeightModifier.kt`
   - 核心方法：`measure()` 方法（第 44-56 行）
   - 作用：自定义布局修饰符，用于调整文本基线的高度，使多个文本元素能够按照基线对齐

2. **AnimatingFabContent**
   - 相对路径：`app/src/main/java/com/example/compose/jetchat/components/AnimatingFabContent.kt`
   - 核心方法：
     - `AnimatingFabContent()` 可组合函数（第 37-99 行）
     - `IconAndTextRow()` 自定义布局实现（第 103-141 行）
   - 作用：创建一个带有展开/收缩动画效果的浮动操作按钮内容布局

3. **JetchatScaffold**
   - 相对路径：`app/src/main/java/com/example/compose/jetchat/components/JetchatScaffold.kt`
   - 核心方法：`JetchatScaffold()` 可组合函数（第 27-34 行）
   - 作用：自定义应用程序的基础布局脚手架

4. **JetchatAppBar**
   - 相对路径：`app/src/main/java/com/example/compose/jetchat/components/JetchatAppBar.kt`
   - 核心方法：`JetchatAppBar()` 可组合函数（第 37-56 行）
   - 作用：自定义应用程序顶部栏

5. **JetchatDrawer**
   - 相对路径：`app/src/main/java/com/example/compose/jetchat/components/JetchatDrawer.kt`
   - 核心方法：`JetchatDrawer()` 可组合函数（第 63-96 行）
   - 作用：自定义抽屉导航组件

这些组件都不是传统的 Android View/ViewGroup 子类，而是使用 Jetpack Compose 的声明式 UI 范式创建的自定义组件。其中，BaselineHeightModifier 是唯一实现了 LayoutModifier 接口的组件，可视为最接近传统自定义 View 的组件。
