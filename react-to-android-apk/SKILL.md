---
name: react-to-android-apk
description: 将 React 网页应用打包成 Android 安装包 (APK)。当用户提到 React、网页应用、移动端打包、APK、Cordova、Capacitor、原生应用包装、或者想要把网站转成手机 App 时触发此技能。
---

# React 网页应用打包为 Android APK

使用 Capacitor 将 React 网页应用转换为原生 Android 安装包。

## 适用场景

- 将现有 React Web App 转换为 Android APK
- 打包网页应用为移动端原生应用
- 不想重写代码即可生成 Android 安装包

## 前置要求

确保用户已安装：
1. **Node.js** (npm 命令)
2. **Android Studio** (用于编译打包)

## 完整步骤

### 第一步：创建本地 React 项目

使用 Vite 创建项目：

```bash
npm create vite@latest vocab-app -- --template react
cd vocab-app
npm install
```

### 第二步：安装项目依赖

```bash
# 安装图标库
npm install lucide-react

# 安装 Tailwind CSS (注意：v4 需要额外安装 postcss 插件)
npm install -D tailwindcss@3 postcss autoprefixer
```

**⚠️ 踩坑记录 (Tailwind v4 兼容问题)**：
- Tailwind CSS v4 对 PostCSS 配置有重大改变
- 如果使用 v4，需要安装 `@tailwindcss/postcss` 并修改配置
- 为避免问题，推荐使用 Tailwind v3：`npm install -D tailwindcss@3`

### 第三步：配置 Tailwind

```bash
# 初始化 Tailwind 配置
npx tailwindcss init -p
```

配置 `tailwind.config.js`:
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: { extend: {} },
  plugins: [],
}
```

配置 `postcss.config.js`:
```javascript
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

在 `src/index.css` 中添加:
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 第四步：集成 Capacitor

```bash
# 安装核心库和命令行工具
npm install @capacitor/core
npm install @capacitor/cli --save-dev

# 初始化 Capacitor
npx cap init
```

初始化时需要提供：
- 应用名称 (App Name)
- 包名 (Bundle ID)，如 `com.qige.vocab`

确保 `capacitor.config.json` 中的 `webDir` 值为 `dist`（Vite 打包产物目录）。

### 第五步：添加 Android 平台

```bash
# 安装 Android 平台支持
npm install @capacitor/android

# 添加 Android 平台到项目
npx cap add android
```

### 第六步：配置国内镜像（必须！）

**⚠️ 踩坑记录 (网络问题)**：
- 国内直接访问 Google/Maven 仓库非常慢，必须配置镜像

1. **配置 Gradle 镜像** - 编辑 `android/gradle/wrapper/gradle-wrapper.properties`:
```properties
distributionUrl=https\://mirrors.cloud.tencent.com/gradle/gradle-8.14.3-all.zip
```

2. **配置 Maven 镜像** - 编辑 `android/build.gradle`:
```groovy
buildscript {
    repositories {
        // 阿里云
        maven { url 'https://maven.aliyun.com/repository/public' }
        maven { url 'https://maven.aliyun.com/repository/google' }
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
        // 华为云
        maven { url 'https://repo.huaweicloud.com/repository/maven/' }
        // 腾讯云
        maven { url 'https://mirrors.cloud.tencent.com/nexus/repository/maven-public/' }
        // 官方仓库
        mavenCentral()
        google()
    }
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
        maven { url 'https://repo.huaweicloud.com/repository/maven/' }
        maven { url 'https://mirrors.cloud.tencent.com/nexus/repository/maven-public/' }
        mavenCentral()
        google()
    }
}
```

### 第七步：降级 AGP 版本

**⚠️ 踩坑记录 (AGP 版本不兼容)**：
- Android Studio 可能不支持最新的 AGP 8.13.0
- 需要降级到兼容版本（推荐 8.8.0）

编辑 `android/build.gradle`:
```groovy
classpath 'com.android.tools.build:gradle:8.8.0'
```

### 第八步：编译并同步

每次修改 React 代码后，执行：

```bash
# 1. 打包 React 网页
npm run build

# 2. 同步到 Android 项目
npx cap sync
```

### 第九步：打包 APK

在 Android Studio 中：
1. 点击 **Sync Project with Gradle Files** 同步项目
2. 等待首次依赖下载完成（可能需要 5-15 分钟）
3. 点击 **Build > Build Bundle(s) / APK(s) > Build APK(s)**
4. 点击右下角弹出的 "locate" 链接找到 `app-debug.apk`

## 常见问题

### Q: npx cap 命令找不到？
A: 使用 `npx cap` 而不是直接运行 node_modules 里的 JS 文件。

### Q: 首次 Gradle 同步非常慢？
A: 正常现象，需要下载 Gradle 运行时和大量依赖。配置好国内镜像后会快很多。

### Q: Build APK 按钮是灰色的？
A: 先执行 Sync Project，等待底部显示 "Build sync finished"。

### Q: 语音朗读 (TTS) 在手机上不工作？
A: 确保手机安装了 Google TTS 引擎或系统自带 TTS。Android WebView 支持 `window.speechSynthesis` API。

## 注意事项

### 沉浸式体验
在 `index.html` 的 `<head>` 中添加：
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

### 网络权限
- Android App 默认开启网络权限
- 调用 Gemini API 等无需额外配置
