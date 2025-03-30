# UserInputTextField 组件分析

## 基本信息

### 传入参数

| 参数 | 类型 | 默认值 | 作用 |
|------|------|--------|------|
| textFieldValue | TextFieldValue | 必填 | 输入框的文本值 |
| onTextChanged | (TextFieldValue) -> Unit | 必填 | 文本变化回调函数 |
| onTextFieldFocused | (Boolean) -> Unit | 必填 | 焦点变化回调函数 |
| keyboardType | KeyboardType | 必填 | 键盘类型 |
| focusState | Boolean | 必填 | 当前输入框的焦点状态 |
| onMessageSent | (String) -> Unit | 必填 | 发送消息的回调函数 |
| modifier | Modifier | Modifier | 修饰符 |

### 主要用途

UserInputTextField 是 Jetchat 应用中消息输入区域的核心组件，用于接收用户输入的聊天消息文本。它提供了基础的文本输入功能，同时包含占位文本提示、焦点管理和发送消息的快捷键支持。作为 BoxScope 的扩展函数，它被设计为在一个 Box 布局容器内使用。

## UI 组件结构

```plaintext
UserInputTextField
├── BasicTextField (核心输入组件)
└── Text (仅在文本为空且无焦点时显示的提示文本)
```

## 状态管理

### 组件内状态

- **lastFocusState**: 使用 `remember { mutableStateOf(false) }` 实现，用于跟踪上一次的焦点状态，避免重复触发焦点变化回调。

### 状态提升

组件采用状态提升模式，大部分状态由父组件维护并通过参数传入：

- `textFieldValue`：文本内容由父组件管理
- `focusState`：焦点状态由父组件管理
- 状态变化通过回调函数 `onTextChanged` 和 `onTextFieldFocused` 通知父组件

这种设计使组件更具可复用性，并允许父组件 (`UserInput`) 协调不同子组件间的状态同步。

## 交互事件

### 可用交互事件

1. **文本输入**：用户可以输入文本内容
2. **焦点变化**：检测输入框的焦点获得和失去
3. **发送消息**：通过键盘的发送按钮触发消息发送

### 事件处理

- **文本变化**：通过 `onValueChange` 回调捕获并使用 `onTextChanged` 传递给父组件
- **焦点变化**：通过 `onFocusChanged` 修饰符捕获，并在状态变化时调用 `onTextFieldFocused`
- **发送消息**：通过 `KeyboardActions` 配置，在用户点击键盘的发送按钮时调用 `onMessageSent`

### 事件传递机制

事件从 BasicTextField 捕获后，通过函数参数回调传递给父组件，形成单向数据流：

1. 用户交互 → BasicTextField
2. BasicTextField → UserInputTextField 回调
3. UserInputTextField → 父组件 (通过回调函数)

## 自定义与样式

### 修饰符支持

组件接受外部传入的 `modifier`，并添加了以下修饰：

- `.padding(start = 32.dp)`：左侧内边距
- `.align(Alignment.CenterStart)`：在 Box 中左侧居中对齐
- `.onFocusChanged`：焦点变化监听

### 主题和样式

- 使用 `LocalTextStyle.current` 和 `LocalContentColor.current` 应用当前主题文本样式
- 提示文本使用 `MaterialTheme.typography.bodyLarge` 样式，颜色为 `onSurfaceVariant`
- 光标颜色（`cursorBrush`）使用当前内容颜色

## 性能考虑

### 重组优化

1. **状态隔离**：通过 `remember` 将内部状态 `lastFocusState` 隔离，避免无关重组时重置
2. **焦点判断优化**：仅在焦点状态实际变化时才调用 `onTextFieldFocused`，减少不必要的回调

### 避免的性能陷阱

1. **避免重复回调**：通过比较 `lastFocusState` 和当前焦点状态，防止频繁触发相同的焦点回调
2. **条件渲染**：提示文本仅在满足条件（文本为空且无焦点）时渲染，避免不必要的视图层级
