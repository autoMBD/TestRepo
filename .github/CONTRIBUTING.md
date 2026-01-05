# Contributing to AMBD-MC

感谢你的关注与贡献！为保证高质量协作，请遵循下列流程。

## 提交流程
- Fork 仓库并在 feature 分支上开发：`git checkout -b feat/your-feature`
- 提交遵循规范（参见下面的 Commit 风格）
- 运行并通过项目测试：`pytest` 或对应语言的 test 命令
- 提交 PR 到 `main`（或 repo 的默认主分支），在 PR 描述中包含复现步骤、改动要点和相关 issue。

## PR Checklist
- [ ] 关联 issue（若有）
- [ ] 新增/修改的代码包含单元测试
- [ ] 通过 CI（lint + tests）
- [ ] 更新或补充必要的文档（README / docs）
- [ ] 变更描述清晰

## 代码风格
- 使用项目推荐的格式化工具（例如 Black / Prettier / gofmt 等）
- 在本地运行 linter 并修复警告

## Commit 信息
建议使用简洁的类型前缀，例如：
```
feat: 新功能
fix: 修复 bug
docs: 文档变更
chore: 构建/工具/依赖变更
refactor: 重构但不改功能
test: 测试相关变更
```

## 发展路线与任务
我们会在仓库的 Projects / Roadmap 中列出中长期计划。若你想成为长期维护者，请在 PR 或 issue 中表明意愿。

谢谢！