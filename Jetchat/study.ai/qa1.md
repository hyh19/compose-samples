# 项目的主入口 Activity 是哪个？请提供其相对路径和关键代码片段位置

项目的主入口 Activity 是 `NavActivity`，相对路径为：

`app/src/main/java/com/example/compose/jetchat/NavActivity.kt`

关键代码片段位置：

1. 类定义（45-48 行）：

    ```kotlin
    /**
    * Main activity for the app.
    */
    class NavActivity : AppCompatActivity() {
    ```

2. AndroidManifest.xml 中的声明（27-36 行）：

    ```xml
    <activity
        android:name=".NavActivity"
        android:windowSoftInputMode="adjustResize"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    ```

3. onCreate 方法（51-92 行）设置了界面布局和导航。
