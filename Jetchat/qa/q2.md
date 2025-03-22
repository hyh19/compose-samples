# AndroidManifest.xml 中声明了哪些关键权限和主题配置？请指出具体行号

AndroidManifest.xml 中声明的关键配置：

1. 主题配置：
   - 第 25 行：`android:theme="@style/Theme.Jetchat.NoActionBar"`
   - 应用使用的是无动作栏的 Jetchat 主题

2. 应用配置：
   - 第 21 行：`android:allowBackup="true"` - 允许应用数据备份
   - 第 22 行：`android:enableOnBackInvokedCallback="true"` - 启用新的返回导航回调
   - 第 23 行：`android:icon="@mipmap/ic_launcher"` - 设置应用图标
   - 第 24 行：`android:label="@string/app_name"` - 设置应用名称
   - 第 25 行：`android:supportsRtl="true"` - 支持从右到左的布局

3. NavActivity 的配置：
   - 第 30 行：`android:windowSoftInputMode="adjustResize"` - 软键盘显示时调整窗口大小
   - 第 31 行：`android:exported="true"` - 允许其他应用启动此 Activity

值得注意的是，该应用并没有声明特殊的权限（如网络访问、相机等），只使用了基本的应用配置。
