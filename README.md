# OpenCode Skills 自动备份

这个仓库自动备份你的 OpenCode Skills 到 GitHub。

## 仓库信息

- **仓库名**: my-skills
- **GitHub**: https://github.com/xsjiasnxj/my-skills
- **同步的技能目录**: C:\Users\liumingwei\.config\opencode\skills

## 自动同步

系统会在每次开机时自动同步你的 skills 到 GitHub。

### 同步脚本位置

- **主脚本**: C:\Users\liumingwei\sync-skills.bat
- **备份目录**: C:\Users\liumingwei\skills-backup

### 手动触发同步

如果需要立即同步，可以：

1. 双击运行 `C:\Users\liumingwei\sync-skills.bat`
2. 或在命令行中执行：
   ```cmd
   C:\Users\liumingwei\sync-skills.bat
   ```

## 包含的技能

- android-studio-gemini - Android Studio Gemini AI 编程指南
- find-skills - 发现和安装技能
- react-to-android-apk - React 打包成 Android APK
- skill-creator - 创建/优化技能
- superpowers - 超级能力技能集合
  - brainstorming
  - systematic-debugging
  - test-driven-development
  - writing-plans
  - executing-plans
  - verification-before-completion
  - requesting-code-review
  - receiving-code-review
  - finishing-a-development-branch
  - subagent-driven-development
  - dispatching-parallel-agents
  - using-git-worktrees
  - using-superpowers
- ui-ux-pro-max - UI/UX 设计

## 注意事项

- 只有当 skills 文件有变化时才会提交新版本
- 启动脚本已添加到 Windows 开机启动项
- 所有提交都会自动推送到 GitHub
