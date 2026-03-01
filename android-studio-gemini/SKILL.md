---
name: android-studio-gemini
description: 在Android Studio中使用Google Gemini AI进行辅助编程。包括：启用Android Studio内置Gemini助手、配置代理解决登录问题、在项目中使用Gemini API Key调用AI功能。适用于想要在Android开发中使用AI辅助、获取API Key集成的开发者。
---

# Android Studio Gemini AI 编程指南

本 skill 指导你在 Android Studio 中使用 Google Gemini 进行 AI 辅助编程。

## 一、启用 Android Studio 内置 Gemini

### 1. 更新 Android Studio

Gemini 从 **Android Studio Iguana（2024版）** 起正式集成。

- **Help → Check for Updates...**
- 确保版本至少为 **Koala** 或更高

### 2. 登录 Google 账号

1. 在 IDE 右上角点击 **"Sign in to Google"**
2. 通过外部浏览器登录
3. 登录成功后，工具栏会出现 **Gemini 图标**

### 3. 代理配置（如有需要）

如果使用代理无法登录：

1. 查看系统代理设置：**设置 → 网络和Internet → 代理**
2. 获取代理 **IP 地址** 和 **端口**
3. 在 Android Studio 中配置：
   - **File → Settings → HTTP Proxy**
   - 选择 **Manual proxy configuration**
   - 类型选择 **HTTP**
   - 填写 Host name 和 Port number

4. 如仍有问题，在代理设置中填写 **Login** 和 **Password**

### 4. 使用 Gemini 助手

- Gemini 入口位于右边栏 **AI Assistant** 面板
- 可以直接对话、生成代码、解释错误

---

## 二、使用 API Key：在代码中调用 Gemini

### 1. 获取 API Key

1. 访问 [Google AI Studio](https://aistudio.google.com/prompts/new_chat)
2. 登录 Google 账号
3. 左侧边栏选择 **Dashboard**
4. 进入 **API Keys** 页面
5. 点击 **Create API Key**
6. 复制生成的 API Key

### 2. 配置项目

在项目根目录的 `local.properties` 中添加：

```properties
GEMINI_API_KEY=your_api_key_here
```

### 3. 添加 Gradle 依赖

在 `android/app/build.gradle` 中加入：

```groovy
dependencies {
    implementation("com.google.ai.client.generativeai:generativeai:<latest-version>)
}
```

### 4. Kotlin 代码调用示例

```kotlin
import com.google.ai.client.generativeai.GenerativeModel

val model = GenerativeModel(
    modelName = "gemini-1.5-flash",
    apiKey = BuildConfig.GEMINI_API_KEY
)

suspend fun askGemini(prompt: String): String {
    val response = model.generateContent(prompt)
    return response.text ?: ""
}
```

### 5. 异步调用

```kotlin
// 在协程中调用
lifecycleScope.launch {
    try {
        val result = askGemini("解释这段代码的作用")
        Log.d("Gemini", result)
    } catch (e: Exception) {
        Log.e("Gemini", "Error: ${e.message}")
    }
}
```

---

## 三、常见问题

| 问题 | 解决方案 |
|------|----------|
| 登录失败/空白页面 | 配置代理，或检查网络环境 |
| Authorization failed | 重新填写代理的 Login 和 Password |
| API 调用失败 | 检查 local.properties 中的 API Key 是否正确 |
| 模型版本选择 | 推荐使用 `gemini-1.5-flash`（免费额度大，速度快） |

---

## 四、免费额度

- **Gemini 1.5 Flash**: 每天 15 次请求（免费）
- 具体额度以 Google 官方为准

