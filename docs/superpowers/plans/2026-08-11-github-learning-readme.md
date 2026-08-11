# GitHub 学习 README 实施计划

> **供智能代理执行：** 必须使用 `superpowers:subagent-driven-development`（推荐）或 `superpowers:executing-plans`，逐项执行本计划。所有步骤使用复选框（`- [ ]`）跟踪进度。

**目标：** 创建、验证、提交并发布一份适合初学者的 GitHub 学习说明文件（`README.md`）。

**架构：** 仓库根目录只使用一个 `README.md` 作为项目首页。进度清单会在第一次推送前后保持与真实状态一致，并通过两次小型 README 提交演示常见的 Git 工作流程。

**技术栈：** Markdown、Git、GitHub

## 全局约束

- README 内容保持简洁，适合初学者阅读。
- 说明文字使用中文，Git 命令保留英文。
- 不得包含未完成的占位内容。
- 每次创建提交时，进度清单必须准确反映当时的仓库状态。

---

## 文件结构

- 创建：`README.md`——项目公开首页，包含项目介绍、学习目标、进度清单和常用命令。
- 参考：`docs/superpowers/specs/2026-08-11-github-learning-readme-design.md`——已经批准的内容设计。

### 任务一：创建并提交学习 README

**涉及文件：**

- 创建：`README.md`

**输入与产出：**

- 输入：已经批准的 README 设计，以及当前 Git 提交历史。
- 产出：能够在 GitHub 正常渲染的 Markdown 首页；“完成第一次提交”为已完成，“推送到 GitHub”为未完成。

- [ ] **步骤 1：创建 README**

创建 `README.md`，内容必须如下：

```markdown
# My First Project

这是我的第一个 GitHub 学习仓库。

## 学习目标

- 了解仓库、分支和提交
- 学会使用 `git add`、`git commit` 和 `git push`
- 学习 GitHub 的基本协作流程

## 当前进度

- [x] 创建 GitHub 仓库
- [x] 连接本地仓库
- [x] 完成第一次提交
- [ ] 推送到 GitHub

## 常用命令

```powershell
git status
git add README.md
git commit -m "Add GitHub learning README"
git push
```
```

- [ ] **步骤 2：验证 README 内容**

运行：

```powershell
Get-Content -Raw -Encoding utf8 -LiteralPath README.md
Select-String -LiteralPath README.md -Pattern '^# My First Project$','^- \[x\] 完成第一次提交$','^- \[ \] 推送到 GitHub$'
git diff --check
git status --short
```

预期结果：

- 终端显示完整 README，所有章节都存在，中文没有乱码。
- `Select-String` 返回标题和两条指定进度记录。
- `git diff --check` 退出码为 0，并且没有空白字符错误。
- `git status --short` 只显示 `?? README.md`。

- [ ] **步骤 3：暂存并检查本次变更**

运行：

```powershell
git add -- README.md
git diff --cached -- README.md
```

预期结果：暂存区（staging area）的差异只包含新建的 `README.md`。

- [ ] **步骤 4：提交 README**

运行：

```powershell
git commit -m "Add GitHub learning README"
```

预期结果：Git 创建一个只包含 `README.md` 的新提交（commit）。

### 任务二：发布提交并记录第一次推送

**涉及文件：**

- 修改：`README.md`

**输入与产出：**

- 输入：已经提交的 README，以及已经配置好的远程仓库 `origin`。
- 产出：GitHub 上的远程 `main` 分支，以及准确记录推送结果的本地 README。

- [ ] **步骤 1：推送当前提交**

运行：

```powershell
git push -u origin main
```

预期结果：Git 将本地 `main` 推送到 `origin`，并让本地 `main` 跟踪 `origin/main`。如果 Git Credential Manager 打开浏览器，请登录已经确认的 GitHub 账号并授权。

- [ ] **步骤 2：验证远程分支**

运行：

```powershell
git status --short --branch
git ls-remote --heads origin main
```

预期结果：

- 状态以 `## main...origin/main` 开头，并且没有未提交的文件变更。
- `git ls-remote` 返回一条包含 `refs/heads/main` 的记录。

- [ ] **步骤 3：把推送状态改为已完成**

将 `README.md` 中的：

```markdown
- [ ] 推送到 GitHub
```

修改为：

```markdown
- [x] 推送到 GitHub
```

- [ ] **步骤 4：验证并提交进度更新**

运行：

```powershell
Select-String -LiteralPath README.md -Pattern '^- \[x\] 推送到 GitHub$'
git diff --check
git add -- README.md
git diff --cached -- README.md
git commit -m "Record first GitHub push"
```

预期结果：

- 命令返回“推送到 GitHub”已完成的记录。
- 没有空白字符错误。
- 暂存差异只修改“推送到 GitHub”这一项的复选框。
- Git 创建一个记录学习进度的新提交。

- [ ] **步骤 5：发布并验证进度更新**

运行：

```powershell
git push
git status --short --branch
git log --oneline --decorate -6
```

预期结果：

- 第二次推送成功。
- 工作区干净，本地 `main` 正在跟踪 `origin/main`。
- 最近六次提交依次包含：推送进度更新、README 创建、计划记录修正、中文实施计划、原始实施计划和 README 设计。
