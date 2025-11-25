# GitHub Actions 自动发布指南

## 概述

本项目已配置 GitHub Actions 工作流，可以自动打包插件并提交 PR 到 Dify Marketplace。

## 前置条件

### 1. Fork dify-plugins 仓库

确保您已经 fork 了 `langgenius/dify-plugins` 仓库到您的 GitHub 账户（`caapap/dify-plugins`）。

### 2. 配置 GitHub Secret

在您的插件仓库（`caapap.github/md-converter`）中配置 GitHub Secret：

1. 进入仓库的 **Settings** → **Secrets and variables** → **Actions**
2. 点击 **New repository secret**
3. 添加以下 Secret：

   **名称**: `PLUGIN_ACTION`
   
   **值**: 一个具有以下权限的 GitHub Personal Access Token (PAT)：
   - `repo` (完整仓库访问权限)
   - `workflow` (更新 GitHub Actions 工作流)
   
   创建 PAT 的步骤：
   - 访问 https://github.com/settings/tokens
   - 点击 **Generate new token** → **Generate new token (classic)**
   - 勾选 `repo` 和 `workflow` 权限
   - 生成并复制 token
   - 将 token 粘贴到 Secret 的值中

## 工作流程

### 自动触发

当以下情况发生时，GitHub Actions 会自动运行：

1. **推送到 main 分支**：每次代码推送到 `main` 分支时
2. **发布 Release**：当您创建一个新的 Release 时

### 工作流步骤

1. **检查代码**：检出当前仓库代码
2. **下载 CLI 工具**：下载最新版本的 Dify 插件打包工具
3. **读取插件信息**：从 `manifest.yaml` 中读取插件名称、版本和作者
4. **打包插件**：使用 CLI 工具打包插件为 `.difypkg` 文件
5. **检出目标仓库**：检出 `caapap/dify-plugins` 仓库
6. **创建分支并提交**：在目标仓库中创建新分支并提交插件包
7. **创建 PR**：向 `langgenius/dify-plugins` 仓库提交 Pull Request

## 使用方法

### 方法 1：推送代码到 main 分支

```bash
# 确保版本号已更新
# 编辑 manifest.yaml，更新 version 字段

# 提交并推送
git add .
git commit -m "Update plugin to version 0.0.5"
git push origin main
```

推送后，GitHub Actions 会自动运行。

### 方法 2：创建 Release

1. 在 GitHub 仓库页面，点击 **Releases** → **Create a new release**
2. 选择或创建新的 tag（例如 `v0.0.5`）
3. 填写 Release 标题和描述
4. 点击 **Publish release**

发布 Release 后，GitHub Actions 会自动运行。

## 检查工作流状态

1. 进入仓库的 **Actions** 标签页
2. 查看最新的工作流运行状态
3. 点击运行记录查看详细日志

## 验证 PR

工作流完成后：

1. 访问 https://github.com/langgenius/dify-plugins/pulls
2. 查找标题为 `bump markitdown plugin to version X.X.X` 的 PR
3. 等待 Dify 团队审核和合并
4. 审核通过后，插件会被签名并发布到 Marketplace

## 注意事项

1. **版本号**：每次发布前，请确保在 `manifest.yaml` 中更新版本号
2. **作者名称**：确保 `manifest.yaml` 中的 `author` 字段与您的 GitHub 用户名一致（当前为 `caapap`）
3. **分支名称**：工作流会自动创建分支，格式为 `bump-markitdown-plugin-{version}`
4. **PR 标题**：PR 标题格式为 `bump markitdown plugin to version {version}`
5. **重复运行**：如果 PR 已存在，工作流会跳过创建步骤

## 故障排除

### 工作流失败

如果工作流失败，请检查：

1. **Secret 配置**：确保 `PLUGIN_ACTION` Secret 已正确配置
2. **权限问题**：确保 PAT 具有足够的权限
3. **仓库访问**：确保已 fork `dify-plugins` 仓库
4. **分支名称冲突**：如果分支已存在，工作流会使用 `--force` 覆盖

### PR 未创建

如果 PR 未创建，可能原因：

1. **分支同步延迟**：工作流已等待 10 秒，但有时需要更长时间
2. **权限不足**：检查 PAT 权限
3. **仓库不存在**：确保 `caapap/dify-plugins` 仓库存在

### 手动触发

如果需要手动触发工作流：

1. 进入 **Actions** 标签页
2. 选择 **Auto Create PR on Main Push** 工作流
3. 点击 **Run workflow**
4. 选择分支（通常是 `main`）
5. 点击 **Run workflow**

## 相关文件

- `.github/workflows/auto-pr.yml` - GitHub Actions 工作流配置
- `manifest.yaml` - 插件清单文件（包含版本和作者信息）

## 更新日志

### v0.0.5
- 添加 LLM 支持（OpenAI/Kimi 兼容）
- 支持自定义模型进行图片描述
- 优化工作流配置

