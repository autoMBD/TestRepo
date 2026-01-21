# Contributing to TestRepo

Thank you for your interest and contributions! To ensure high-quality collaboration, please follow the process below.

## Submission Process
- Fork the repository and develop on a feature branch: `git checkout -b feat/your-feature`
- Submissions follow the standards (see Commit Style below)
- Run and pass project tests: `pytest` or the corresponding language's test command
- Submit a PR to `main` (or the repo's default main branch), including reproduction steps, key changes, and related issues in the PR description.

## PR Checklist
- [ ] Link to issue (if any)
- [ ] New/modified code includes unit tests
- [ ] Pass CI (lint + tests)
- [ ] Update or supplement necessary documentation (README / docs)
- [ ] Changes described clearly

## Code Style
- Use the project's recommended formatting tools (e.g., Black / Prettier / gofmt)
- Run linter locally and fix warnings

## Commit Messages
We recommend using concise type prefixes, such as:
```
feat: new feature
fix: fix bug
docs: documentation changes
chore: build/tool/dependency changes
refactor: refactor without changing functionality
test: test-related changes
```

## Development Roadmap and Tasks
We will list medium- to long-term plans in the repository's Projects / Roadmap. If you want to become a long-term maintainer, please express your interest in a PR or issue.

Thank you!