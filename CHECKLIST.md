# 发布前检查清单

在通过 GitHub Actions 发布插件之前，请确认以下事项：

## ✅ 必需配置

- [ ] **GitHub Secret 已配置**
  - [ ] 在仓库 Settings → Secrets → Actions 中添加了 `PLUGIN_ACTION`
  - [ ] Secret 值是一个有效的 GitHub Personal Access Token
  - [ ] PAT 具有 `repo` 和 `workflow` 权限

- [ ] **已 Fork dify-plugins 仓库**
  - [ ] 已 fork `langgenius/dify-plugins` 到 `caapap/dify-plugins`
  - [ ] Fork 的仓库是公开的

- [ ] **manifest.yaml 配置正确**
  - [ ] `version` 字段已更新（当前应为 `0.0.5`）
  - [ ] `author` 字段为 `caapap`（与 GitHub 用户名一致）
  - [ ] `created_at` 时间正确（不能是未来时间）

## ✅ 代码检查

- [ ] **所有文件已提交**
  - [ ] `tools/markitdown.py` - 包含 LLM 支持
  - [ ] `tools/markitdown.yaml` - 包含 LLM 参数配置
  - [ ] `requirements.txt` - 包含 `openai` 依赖
  - [ ] `manifest.yaml` - 版本号已更新
  - [ ] `.github/workflows/auto-pr.yml` - 工作流配置正确

- [ ] **功能测试**
  - [ ] 插件在本地调试模式下可以正常运行
  - [ ] 基本文件转换功能正常
  - [ ] LLM 配置参数正确（如果测试了 LLM 功能）

## ✅ 发布步骤

1. **更新版本号**（如果还没有）
   ```bash
   # 编辑 manifest.yaml，更新 version 字段
   version: 0.0.5
   ```

2. **提交并推送**
   ```bash
   git add .
   git commit -m "Release version 0.0.5 with LLM support"
   git push origin main
   ```

3. **监控工作流**
   - 进入仓库的 **Actions** 标签页
   - 查看工作流运行状态
   - 等待工作流完成（通常需要 1-2 分钟）

4. **验证 PR**
   - 访问 https://github.com/langgenius/dify-plugins/pulls
   - 查找新创建的 PR
   - 确认插件包已正确上传

5. **等待审核**
   - Dify 团队会审核 PR
   - 审核通过后，插件会被签名并发布到 Marketplace

## 📝 发布后

- [ ] 在 Dify Marketplace 中验证插件已发布
- [ ] 测试从 Marketplace 安装插件
- [ ] 确认签名验证通过
- [ ] 更新 README 或文档（如需要）

## 🐛 如果遇到问题

1. **工作流失败**
   - 检查 Actions 日志
   - 确认 Secret 配置正确
   - 确认已 fork dify-plugins 仓库

2. **PR 未创建**
   - 检查 PAT 权限
   - 确认分支名称没有冲突
   - 查看工作流日志中的错误信息

3. **签名验证失败**
   - 这是正常的，只有通过 Marketplace 发布的插件才有官方签名
   - 本地测试请使用调试模式（见 DEBUG.md）

## 📚 相关文档

- `GITHUB_ACTIONS.md` - 详细的 GitHub Actions 使用指南
- `DEBUG.md` - 本地调试模式使用指南
- `README.md` - 插件功能说明

