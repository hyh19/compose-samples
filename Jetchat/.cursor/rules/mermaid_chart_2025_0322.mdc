---
description: 确保所有图表绘制需求使用 Mermaid 语法规范实现
globs: ["**/*.md"]
alwaysApply: true
---
# Mermaid 图表规范

## 描述

此规则文件用于确保所有需要绘制图表的需求使用 Mermaid 语法规范实现，提高文档中图表的一致性、可维护性和可读性。

## 应用条件

当处理以下类型的文件时应用此规则：

- **/*.md

## 规则内容

在处理需要图表的文档时，请严格遵循以下规范：

### 1. 基本要求

- 所有需要绘制图表的需求，必须使用 Mermaid 语法实现
- 避免使用截图、外部图片链接或其他图表工具
- Mermaid 代码块应使用正确的标记：\```mermaid 和 \```

### 2. 图表类型选择

- 根据图表内容选择最合适的图表类型：
  - 流程图（flowchart）：表示工作流程、算法流程
  - 序列图（sequence diagram）：表示对象间的交互
  - 类图（class diagram）：表示系统结构和类关系
  - 状态图（state diagram）：表示状态转换
  - 甘特图（gantt）：表示项目计划和时间线
  - 饼图（pie）：表示比例数据
  - 关系图（ER diagram）：表示数据模型关系

### 3. 样式与格式

- 使用清晰的节点标识和描述性标签
- 保持图表视觉简洁，避免过度复杂
- 使用一致的样式和颜色方案
- 为复杂节点添加合适的形状和颜色以增强可读性
- 确保节点和关系之间的连线清晰可辨

### 4. 注释与说明

- 在 Mermaid 代码前添加必要的说明文字
- 复杂图表应增加图例或说明
- 重要节点或关系应在图表下方补充解释

### 5. 示例

下面是一个简单流程图示例：

```mermaid
flowchart TD
    A[开始] --> B{是否满足条件?}
    B -->|是| C[处理步骤1]
    B -->|否| D[处理步骤2]
    C --> E[结束]
    D --> E
```

## 参考资料

- [Mermaid 官方文档](https://mermaid-js.github.io/mermaid/#/)
- [Mermaid 在线编辑器](https://mermaid-js.github.io/mermaid-live-editor)
- [Markdown 中使用 Mermaid 的最佳实践](https://mermaid-js.github.io/mermaid/#/integrations)
