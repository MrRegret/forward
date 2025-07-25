# GitHub Actions工作流修复计划

## 上下文
用户反馈GitHub Actions工作流工作不正常，需要修复现有的自动更新功能。

## 问题分析
1. **版本号冲突**：`update.sh`和Actions都在更新版本号，可能导致冲突
2. **权限问题**：可能缺少必要的仓库权限配置
3. **错误处理**：缺少失败时的回滚机制和详细错误信息
4. **日志不足**：难以诊断失败原因，需要更详细的调试信息

## 修复计划

### 步骤1：修复版本号更新逻辑
- **文件**：`scripts/update.sh`
- **操作**：移除版本号自动更新逻辑，改为由Actions统一管理
- **预期结果**：避免版本号冲突，确保版本管理的一致性

### 步骤2：优化Actions权限配置
- **文件**：`.github/workflows/sync-repos.yml`, `.github/workflows/manual-update.yml`
- **操作**：
  - 添加必要的仓库权限（contents: write, actions: read, metadata: read）
  - 配置正确的token权限范围
  - 添加权限验证步骤
- **预期结果**：确保Actions有足够权限执行所有操作

### 步骤3：增强错误处理机制
- **文件**：`.github/workflows/sync-repos.yml`, `.github/workflows/manual-update.yml`
- **操作**：
  - 添加每个步骤的错误检查
  - 实现失败时的回滚机制
  - 添加条件执行逻辑
  - 设置合理的超时时间
- **预期结果**：提高工作流的稳定性和可靠性

### 步骤4：改进日志和调试信息
- **文件**：`.github/workflows/sync-repos.yml`, `.github/workflows/manual-update.yml`
- **操作**：
  - 添加详细的步骤日志输出
  - 增加调试信息和状态检查
  - 优化错误消息格式
  - 添加执行时间统计
- **预期结果**：便于问题诊断和性能监控

### 步骤5：优化提交和标签流程
- **文件**：`.github/workflows/sync-repos.yml`, `.github/workflows/manual-update.yml`
- **操作**：
  - 改进提交消息格式
  - 优化标签创建逻辑
  - 添加提交前的最终检查
  - 确保原子性操作
- **预期结果**：提高提交质量和版本管理的准确性

### 步骤6：测试和验证
- **操作**：
  - 手动触发工作流测试
  - 验证版本号更新逻辑
  - 检查提交和标签创建
  - 确认所有功能正常工作
- **预期结果**：确保修复后的工作流完全正常

### 步骤7：更新版本号并提交
- **文件**：`package.json`
- **操作**：版本号从v1.1.4升级到v1.1.5
- **预期结果**：记录本次修复的版本变更

## 技术要点
- 使用`set -e`确保脚本在错误时退出
- 合理使用`if`条件判断避免不必要的操作
- 添加`continue-on-error`选项处理非关键步骤
- 使用`GITHUB_OUTPUT`正确传递步骤间的数据
- 确保所有文件路径和命令的正确性