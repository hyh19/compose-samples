# ClickableMessage 组件分析

## 基本信息

### 参数列表

| 参数名 | 类型 | 默认值 | 作用 |
|-------|------|-------|------|
| message | Message | 无默认值，必需参数 | 要显示的消息对象，包含内容和作者等信息 |
| isUserMe | Boolean | 无默认值，必需参数 | 标识消息是否由当前用户发送，影响消息的样式 |
| authorClicked | (String) -> Unit | 无默认值，必需参数 | 点击消息中提及的用户名时的回调函数 |

### 主要用途和适用场景

ClickableMessage 组件用于在聊天界面中渲染可点击的消息文本内容，支持 Markdown 风格的格式化和特殊类型内容（链接、@用户名）的交互。它是聊天气泡内部的核心内容展示组件，主要用于富文本消息的显示和交互处理。

## UI 组件结构

```plaintext
ClickableMessage
└── ClickableText (内部包含格式化后的文本内容)
```

## 状态管理

### 组件状态

- 组件本身没有内部状态管理，是一个无状态组件
- 通过参数传入所有必要的信息来确定渲染结果

### 状态影响 UI 更新

- `isUserMe` 参数传递给 `messageFormatter` 函数，决定文本的样式（如链接和代码片段的颜色）
- 格式化后的 `styledMessage` 作为最终渲染内容提供给 ClickableText

### 状态提升

- 消息内容、发送者信息以及点击回调都从父组件传入，实现了完全的状态提升
- 这使得组件更具可复用性，父组件可以控制消息的外观和交互行为

## 交互事件

### 可用的交互事件

1. 点击链接：检测并处理消息中的 HTTP/HTTPS 链接
2. 点击用户名：检测并处理消息中的 @用户名标记
3. 点击其他格式化文本：虽然可点击，但没有特殊行为

### 事件监听与处理

- 使用 `ClickableText` 的 `onClick` 属性监听点击事件
- 通过 `getStringAnnotations` 获取点击位置的注解信息
- 根据注解类型（LINK 或 PERSON）执行不同的操作：
  - LINK：使用 `LocalUriHandler` 打开链接
  - PERSON：调用 `authorClicked` 回调函数

### 事件传递机制

- 文本点击事件由 ClickableText 捕获
- 事件处理逻辑集中在 onClick lambda 中
- 特定类型的交互（如打开链接或查看用户资料）委托给相应的系统组件或父组件处理

### 最佳实践

- 将不同类型的点击行为通过注解系统分离，提高代码可维护性
- 使用 `firstOrNull()?.let {}` 优雅处理可能的空值情况
- 将打开链接等行为委托给系统组件，避免重复实现

## 自定义与样式

### 组件外观自定义

- 文本样式通过 `MaterialTheme.typography.bodyLarge` 设置，并结合 `LocalContentColor.current` 应用正确的颜色
- 通过 `Modifier.padding(16.dp)` 为文本添加内边距
- 使用 `messageFormatter` 函数处理特殊格式标记（如 `*粗体*`, `_斜体_`, `~删除线~`, `` `代码` ``）

### 支持的修饰符

- 组件接受并传递基本的修饰符，如 `padding`
- 没有使用复杂的修饰符链，保持组件的简洁性

### 主题相关配置

- 利用 MaterialTheme 提供的排版样式和颜色方案
- 使用 `LocalContentColor.current` 获取当前上下文的内容颜色
- 基于 `isUserMe` 参数调整特定元素（如链接、代码片段）的颜色

### Material Design 相关属性

- 遵循 Material Design 的排版规范
- 使用 Material 3 的 `MaterialTheme.typography` 和 `MaterialTheme.colorScheme`

## 性能考虑

### 重组影响

- 作为无状态组件，只有在参数变化时才会重组
- `messageFormatter` 调用在每次重组时都会执行，可能成为性能瓶颈

### 性能优化技巧

- `messageFormatter` 返回值可以考虑用 `remember` 缓存，减少不必要的重复格式化处理
- 可以考虑将 `LocalUriHandler.current` 提取到组合函数之外，减少重组开销

### 避免的性能陷阱

- 避免在 onClick 处理程序中执行复杂的操作，特别是 UI 相关的操作
- 注意大型文本的格式化可能带来的性能影响，特别是复杂的正则表达式匹配
- 对于大量消息的场景，应确保父组件使用 LazyColumn 进行高效渲染
