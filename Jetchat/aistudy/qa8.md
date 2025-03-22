# 项目如何适配不同屏幕尺寸和分辨率？请指出相关资源文件和代码实现位置

Jetchat 项目使用多种技术实现对不同屏幕尺寸和分辨率的适配，主要包括以下几个方面：

## 1. 基础适配资源文件

**布局维度定义**：

- **路径**：`app/src/main/res/values/dimens.xml`
- **实现**：定义了基础的边距和尺寸值，如 `activity_horizontal_margin`、`nav_header_spacing` 等

## 2. 自适应布局实现

### BoxWithConstraints 约束盒子

**主要位置**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/profile/Profile.kt`（第87-110行）
- **实现方式**：使用 `BoxWithConstraints` 获取容器尺寸，并基于这些约束动态调整 UI 元素：

  ```kotlin
  BoxWithConstraints(
      modifier = Modifier
          .fillMaxSize()
          .nestedScroll(nestedScrollInteropConnection)
          .systemBarsPadding()
  ) {
      // 使用 this@BoxWithConstraints.maxHeight 访问容器高度
      ProfileHeader(scrollState, userData, this@BoxWithConstraints.maxHeight)
      UserInfoFields(userData, this@BoxWithConstraints.maxHeight)
  }
  ```

### 相对尺寸与约束适配

**用户信息区域**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/profile/Profile.kt`（第130-150行）
- **实现**：根据容器高度动态计算填充空间，确保在不同屏幕上的显示效果一致：

  ```kotlin
  // 添加一个固定显示区域，确保不同设备上始终至少显示部分内容
  Spacer(Modifier.height((containerHeight - 320.dp).coerceAtLeast(0.dp)))
  ```

**头像图片适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/profile/Profile.kt`（第190-200行）
- **实现**：使用 `heightIn` 限制图片最大高度为容器高度的一半：

  ```kotlin
  Image(
      modifier = Modifier
          .heightIn(max = containerHeight / 2)
          .fillMaxWidth()
          // ...
  )
  ```

## 3. 系统 UI 适配

**状态栏与导航栏适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/NavActivity.kt`（第50-57行）
- **实现**：使用 `enableEdgeToEdge()` 和 `consumeWindowInsets = false` 实现沉浸式体验

**抽屉布局适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/components/JetchatDrawer.kt`（第69-72行）
- **实现**：使用 `windowInsetsTopHeight` 适配状态栏高度：

  ```kotlin
  // 使用 windowInsetsTopHeight() 添加一个推动抽屉内容的间隔器
  // 使其位于状态栏下方
  Spacer(Modifier.windowInsetsTopHeight(WindowInsets.statusBars))
  ```

**软键盘适配**：

- **路径**：`app/src/main/AndroidManifest.xml`（第29行）
- **实现**：配置 `android:windowSoftInputMode="adjustResize"` 实现键盘弹出时的布局自适应

**会话界面适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/conversation/Conversation.kt`（第179-182行）
- **实现**：配置 `contentWindowInsets` 排除导航栏和输入法区域：

  ```kotlin
  contentWindowInsets = ScaffoldDefaults
      .contentWindowInsets
      .exclude(WindowInsets.navigationBars)
      .exclude(WindowInsets.ime)
  ```

## 4. 响应式尺寸定义

**输入组件适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/conversation/UserInput.kt`（第670行）
- **实现**：使用 `sizeIn` 定义最小宽高约束：

  ```kotlin
  .sizeIn(minWidth = 42.dp, minHeight = 42.dp)
  ```

**文本样式适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/theme/Typography.kt`
- **实现**：使用 `sp` 单位定义可随系统字体大小调整的文本尺寸

## 5. 可组合函数内部约束

**列表适配**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/conversation/Conversation.kt`（第297行）
- **实现**：使用 `fillMaxSize()` 让 UI 组件填充可用空间

**个人资料列表按钮**：

- **路径**：`app/src/main/java/com/example/compose/jetchat/profile/Profile.kt`（第251行）
- **实现**：使用 `widthIn(min = 48.dp)` 确保按钮的最小可点击区域

这个项目没有使用传统的 XML 密度限定符（如 hdpi、xhdpi 等）进行适配，而是主要利用 Jetpack Compose 的声明式 UI 和自适应布局系统，通过代码来动态计算和适应不同屏幕尺寸，这是更现代的适配方式。
