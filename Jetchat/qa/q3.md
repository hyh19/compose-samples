# 项目包含哪些主要界面（Activity/Fragment/Composable）？请提供所有关键界面的类名及其相对路径

根据代码分析，Jetchat 项目主要包含以下界面组件：

#### 主要 Activity

- **NavActivity**
  - 路径：`app/src/main/java/com/example/compose/jetchat/NavActivity.kt`
  - 职责：应用的主入口点

#### 主要 Fragment

- **ConversationFragment**
  - 路径：`app/src/main/java/com/example/compose/jetchat/conversation/ConversationFragment.kt`
  - 职责：聊天对话界面的容器
- **ProfileFragment**
  - 路径：`app/src/main/java/com/example/compose/jetchat/profile/ProfileFragment.kt`
  - 职责：用户资料界面的容器

#### 主要 Composable 界面

- **ConversationContent**
  - 路径：`app/src/main/java/com/example/compose/jetchat/conversation/Conversation.kt`
  - 职责：聊天对话的主要 Compose 界面
- **UserInput**
  - 路径：`app/src/main/java/com/example/compose/jetchat/conversation/UserInput.kt`
  - 职责：消息输入区域的 Compose 界面
- **ProfileScreen**
  - 路径：`app/src/main/java/com/example/compose/jetchat/profile/Profile.kt`
  - 职责：用户资料的主要 Compose 界面
- **JetchatDrawer**
  - 路径：`app/src/main/java/com/example/compose/jetchat/components/JetchatDrawer.kt`
  - 职责：导航抽屉的 Compose 界面

#### 小部件相关

- **JetChatWidget**
  - 路径：`app/src/main/java/com/example/compose/jetchat/widget/JetChatWidget.kt`
  - 职责：应用的主屏幕小部件实现

这些组件共同构成了 Jetchat 的完整界面，实现了聊天应用的核心功能。应用采用了现代的架构模式，将传统的 Fragment 和 Jetpack Compose 结合使用，通过 Navigation 组件管理导航流程。
