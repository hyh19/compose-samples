# JetchatDrawer 组件分析

## 1. 基本信息

### 参数说明

| 参数名 | 类型 | 默认值 | 作用 |
|-------|------|-------|------|
| drawerState | DrawerState | rememberDrawerState(initialValue = Closed) | 控制抽屉的状态，如打开或关闭 |
| selectedMenu | String | 无（必须提供） | 当前选中的菜单项，用于高亮显示 |
| onProfileClicked | (String) -> Unit | 无（必须提供） | 点击个人资料项时的回调函数 |
| onChatClicked | (String) -> Unit | 无（必须提供） | 点击聊天项时的回调函数 |
| content | @Composable () -> Unit | 无（必须提供） | 抽屉外的主要内容区域 |

### 主要用途和适用场景

`JetchatDrawer` 是一个导航抽屉组件，基于 Material3 的 `ModalNavigationDrawer` 构建。它主要用于：

- 提供应用的主要导航功能
- 显示聊天频道列表
- 展示用户个人资料入口
- 作为应用的主界面骨架，容纳其他内容

适用于需要侧边导航的聊天应用、社交媒体或内容丰富的应用程序。

## 2. UI 组件结构

```plaintext
JetchatDrawer
├── JetchatTheme
    └── ModalNavigationDrawer
        ├── drawerContent
        │   └── ModalDrawerSheet
        │       └── JetchatDrawerContent
        │           ├── Spacer (状态栏高度)
        │           ├── DrawerHeader
        │           │   ├── JetchatIcon
        │           │   └── Image (Logo)
        │           ├── DividerItem
        │           ├── DrawerItemHeader ("Chats")
        │           ├── ChatItem ("composers")
        │           ├── ChatItem ("droidcon-nyc")
        │           ├── DividerItem
        │           ├── DrawerItemHeader ("Recent Profiles")
        │           ├── ProfileItem (当前用户)
        │           ├── ProfileItem (同事)
        │           ├── DividerItem (可选)
        │           ├── DrawerItemHeader ("Settings") (可选)
        │           └── WidgetDiscoverability (可选)
        └── content (传入的主内容)
```

## 3. 状态管理

### 包含的状态

- **drawerState**：控制抽屉是否打开，使用 `rememberDrawerState` 创建和管理
- **selectedMenu**：当前选中的菜单项，作为参数传入，实现状态提升

### 状态 API 使用

- `rememberDrawerState`：用于创建和记忆抽屉状态，默认为关闭
- 没有直接在组件内使用 `remember` 或 `mutableStateOf`，而是通过参数传递状态

### 状态影响 UI 更新

- `drawerState` 控制抽屉的开关状态
- `selectedMenu` 决定哪个菜单项被高亮显示（通过改变背景色和文本颜色）

### 状态提升

- `drawerState`、`selectedMenu`、`onProfileClicked` 和 `onChatClicked` 都是从父组件传入的，体现了状态提升模式
- 这使得组件本身无状态化，便于复用和测试
- 状态变化和事件处理的逻辑由调用方控制

## 4. 交互事件

### 可用的交互事件

- 点击聊天项（通过 `onChatClicked` 回调处理）
- 点击个人资料项（通过 `onProfileClicked` 回调处理）
- 抽屉的滑动打开/关闭（通过 `drawerState` 管理）

### 事件监听与处理

- 点击事件通过 `ChatItem` 和 `ProfileItem` 组件内的 `clickable` 修饰符实现
- 事件处理通过回调函数传递给父组件

### 事件传递机制

- 采用回调函数方式将事件从子组件传递到父组件
- 遵循单向数据流，事件向上传递，状态向下传递

### 最佳实践

- 使用类型安全的函数参数定义回调
- 清晰分离 UI 和事件处理逻辑
- 使用参数默认值简化调用

## 5. 自定义与样式

### 组件外观自定义

- 通过 `JetchatTheme` 包装整个组件，确保应用一致的主题
- 使用 Material3 的 `ModalDrawerSheet` 提供材料设计外观

### 支持的修饰符

- 直接支持的修饰符较少，主要通过内部组件实现样式定制
- 由内部的 `ModalNavigationDrawer` 和 `ModalDrawerSheet` 组件支持修饰符

### 主题相关配置

- 使用 `JetchatTheme` 应用自定义主题
- 使用 `MaterialTheme.colorScheme` 获取颜色
  - `background`：抽屉背景色
  - `onBackground`：抽屉内容文本色

### Material Design 相关属性

- 遵循 Material3 设计规范
- 使用 `ModalNavigationDrawer` 实现导航抽屉
- 适当使用颜色、排版和间距

## 6. 性能考虑

### 重组影响

- 当 `drawerState`、`selectedMenu` 或回调函数发生变化时会触发重组
- 嵌套的 `content` 内容变化也会导致重组

### 性能优化技巧

- 使用 `rememberDrawerState` 避免不必要的状态重建
- `JetchatDrawerContent` 作为一个独立组件，仅在相关参数变化时重组

### 避免的性能陷阱

- 避免在 `JetchatDrawer` 内部使用复杂的计算或状态管理
- 避免传递过多不必要的参数，这会增加重组成本
- 不要在抽屉内容中放置大量复杂组件，会影响滑动性能
