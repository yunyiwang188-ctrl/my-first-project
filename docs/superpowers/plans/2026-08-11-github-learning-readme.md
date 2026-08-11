# GitHub Learning README Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create, validate, commit, and publish a beginner-friendly GitHub learning README.

**Architecture:** The repository will use one root-level `README.md` as its public landing page. Its progress checklist will reflect the repository state before and after the first push, producing two small README commits that demonstrate the normal Git workflow.

**Tech Stack:** Markdown, Git, GitHub

## Global Constraints

- Keep the README beginner-friendly and concise.
- Use Chinese explanatory text with standard Git command names.
- Include no placeholders.
- Keep the progress checklist accurate at the moment each commit is created.

---

## File Structure

- Create: `README.md` — public project introduction, learning goals, progress checklist, and command reference.
- Existing reference: `docs/superpowers/specs/2026-08-11-github-learning-readme-design.md` — approved content design.

### Task 1: Create and commit the learning README

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: the approved README design and the current Git history.
- Produces: a GitHub-renderable Markdown landing page with the first commit marked complete and the first push marked incomplete.

- [ ] **Step 1: Create the README**

Create `README.md` with exactly this content:

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

- [ ] **Step 2: Validate the README content**

Run:

```powershell
Get-Content -Raw -LiteralPath README.md
Select-String -LiteralPath README.md -Pattern '^# My First Project$','^- \[x\] 完成第一次提交$','^- \[ \] 推送到 GitHub$'
git diff --check
git status --short
```

Expected:

- The full README renders in the terminal without missing sections.
- All three required lines are returned by `Select-String`.
- `git diff --check` exits with code 0 and prints no whitespace errors.
- `git status --short` lists only `?? README.md`.

- [ ] **Step 3: Stage and inspect the intended change**

Run:

```powershell
git add -- README.md
git diff --cached -- README.md
```

Expected: the staged diff contains only the new `README.md`.

- [ ] **Step 4: Commit the README**

Run:

```powershell
git commit -m "Add GitHub learning README"
```

Expected: Git creates one commit containing `README.md`.

### Task 2: Publish the commits and record the successful push

**Files:**
- Modify: `README.md`

**Interfaces:**
- Consumes: the committed README and configured `origin` remote.
- Produces: a remote `main` branch plus an updated local progress checklist.

- [ ] **Step 1: Push the current commits**

Run:

```powershell
git push -u origin main
```

Expected: Git pushes `main` to `origin` and configures `main` to track `origin/main`. If Git Credential Manager opens a browser, sign in to the confirmed GitHub account and authorize the operation.

- [ ] **Step 2: Verify the remote branch**

Run:

```powershell
git status --short --branch
git ls-remote --heads origin main
```

Expected:

- Status begins with `## main...origin/main` and shows no file changes.
- `git ls-remote` returns one `refs/heads/main` line.

- [ ] **Step 3: Mark the push complete**

Change this line in `README.md`:

```markdown
- [ ] 推送到 GitHub
```

to:

```markdown
- [x] 推送到 GitHub
```

- [ ] **Step 4: Validate and commit the progress update**

Run:

```powershell
Select-String -LiteralPath README.md -Pattern '^- \[x\] 推送到 GitHub$'
git diff --check
git add -- README.md
git diff --cached -- README.md
git commit -m "Record first GitHub push"
```

Expected:

- The completed push line is returned.
- No whitespace errors are reported.
- The staged diff changes only the push checkbox.
- Git creates one progress-update commit.

- [ ] **Step 5: Publish and verify the progress update**

Run:

```powershell
git push
git status --short --branch
git log --oneline --decorate -3
```

Expected:

- The second push succeeds.
- The working tree is clean and `main` tracks `origin/main`.
- The latest three commits are the progress update, README creation, and README design.
