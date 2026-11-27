# GitHub 练习清单

共 5 条练习，难度由浅入深。建议按顺序完成，每条都给出「目标 / 步骤 / 验收标准 / 提示」，完成后在本文件底部勾选。

---

## 练习 1（入门）：仓库、提交与 README

**目标**：熟悉仓库的基本生命周期，理解工作区、暂存区、本地仓库、远程仓库四层关系。

**步骤**
1. 在 GitHub 上新建一个公开仓库 `github-practice`，初始化时勾选 `Add a README file`。
2. 在本地克隆：`git clone git@github.com:<你的用户名>/github-practice.git`。
3. 新建文件 `hello.md`，写一行自我介绍。
4. 执行 `git status` 观察文件状态，然后 `git add hello.md` 再看一次。
5. 提交并推送：`git commit -m "docs: add hello.md"`，`git push origin main`。
6. 回到网页端，编辑 README 加一句项目简介，再在本地 `git pull` 把这次改动拉回来。

**验收标准**
- 网页端仓库里能看到 `hello.md` 与更新后的 README。
- 本地 `git log --oneline` 至少 2 条提交，且 `git status` 显示 clean。

**提示**
- 提交信息推荐 `type: subject` 格式（feat / fix / docs / chore）。
- 遇到 `Permission denied (publickey)` 说明 SSH key 没配好，改用 HTTPS 或先配置密钥。

---

## 练习 2（基础）：分支、合并与 Pull Request

**目标**：掌握特性分支工作流，能独立发起并合并一个 Pull Request。

**步骤**
1. 从 `main` 拉出新分支：`git switch -c feature/profile`。
2. 在 `hello.md` 中补充一段「我的技能」列表。
3. 分两次提交，每次只改一件事，体会小步提交。
4. `git push -u origin feature/profile` 推到远程。
5. 在网页端创建 PR，标题写清楚做了什么，描述里用 `- [ ]` 列出待办清单。
6. 自己给 PR 留一条 review comment，再自己 resolve 掉。
7. 用 `Squash and merge` 合并，删除远程分支。
8. 本地切回 main，`git pull`，再删除本地分支 `git branch -d feature/profile`。

**验收标准**
- PR 有描述、有评论记录，状态为 Merged。
- main 上出现一条合并提交，`git branch -a` 里看不到 `feature/profile`。

**提示**
- PR 描述里写 `Closes #<issue号>` 可以在合并时自动关闭对应 issue（练习 3 会用到）。
- 合并前先 `git fetch && git log main..feature/profile` 确认改了什么。

---

## 练习 3（进阶）：Issue 驱动与 Projects 看板

**目标**：用 Issue 拆解需求，用看板管理状态，把代码改动和任务追踪串起来。

**步骤**
1. 新建 3 个 Issue，分别描述一个小需求（例如「增加贡献指南」「补充 License」「加一条练习 6」），并打上 `enhancement` 之类的标签。
2. 新建一个 GitHub Projects（Board 视图），把 3 个 Issue 加进去，列设为 `Todo / In Progress / Done`。
3. 新建一个 Issue 模板：`.github/ISSUE_TEMPLATE/bug_report.md`，包含复现步骤与期望行为。
4. 挑一个 Issue，按练习 2 的流程开分支、改代码、发 PR，PR 描述里写 `Closes #1`。
5. 观察 Issue 被自动关闭、看板卡片自动移动到 Done。
6. 给剩下的 Issue 分别设置 assignee 和 milestone。

**验收标准**
- 看板上 3 张卡片状态各不相同，且至少 1 个 Issue 因 PR 合并而自动关闭。
- 仓库里存在 `ISSUE_TEMPLATE/bug_report.md`，新建 Issue 时能看到模板入口。

**提示**
- 看板可以配置自动工作流（如「PR 合并后移到 Done」），比手动拖拽省事。
- Issue 标题同样建议带前缀，便于搜索筛选。

---

## 练习 4（高级）：GitHub Actions 持续集成

**目标**：为仓库添加一个真正会跑起来的 CI 工作流，理解触发条件、作业与步骤。

**步骤**
1. 在仓库根目录新建 `.github/workflows/ci.yml`（注意：本仓库该目录为受保护路径，练习在自己的 `github-practice` 仓库里做）。
2. 配置触发条件：`push` 到任意分支 + `pull_request` 到 `main`。
3. 写一个 `lint` 作业：`actions/checkout@v4` → 安装运行时 → 跑一个最简单的检查（例如 `markdownlint` 或 `echo` + 文件存在性校验）。
4. 再写一个 `test` 作业，用 `needs: lint` 让它依赖前一个作业。
5. 故意提交一次会失败的内容，在 Actions 页面观察失败日志，然后修复并让它变绿。
6. 在仓库 Settings → Branches 里给 `main` 加保护规则：要求 CI 通过 + 至少 1 个 approve 才能合并。
7. 开一个 PR，确认红灯时无法合并，修复后变绿才能合并。

**验收标准**
- Actions 页面能看到至少 1 次失败、1 次成功的运行记录。
- main 分支保护规则生效，PR 上出现 required check。

**提示**
- 工作流 YAML 对缩进极其敏感，两个空格一级，别用 Tab。
- 本地想验证可用 `act`，但最可靠的还是推到远程看真实运行结果。
- 失败日志先看第一步失败还是最后一步失败，往往能直接定位问题。

---

## 练习 5（综合）：开源协作全流程

**目标**：模拟一次真实的开源贡献，涵盖 Fork、上游同步、变基、冲突解决与版本发布。

**步骤**
1. 找一个练习用的公开仓库（或让同学开一个）作为上游，Fork 到自己的账号。
2. 克隆自己的 Fork，添加上游：`git remote add upstream <上游仓库地址>`。
3. 让上游 `main` 前进几次提交，本地执行：
   `git fetch upstream` → `git switch main` → `git rebase upstream/main` → `git push --force-with-lease`。
   体会 Fork 与上游同步的完整过程。
4. 基于最新 main 开分支，改一处文档，向上游发起 PR（`upstream/main` ← 你的分支）。
5. 让上游的维护者（或自己造一次）在 main 上改动同一段落，制造冲突。
6. 本地 `git rebase upstream/main` 复现冲突，手动解决后 `git rebase --continue`，确认 `git log` 历史是线性的。
7. 上游合并你的 PR 后，回到自己的 Fork：`git switch main && git pull upstream main && git push origin main`。
8. 在 GitHub 上为上游仓库打一个 tag（如 `v1.1.0`）并创建 Release，说明本次变更加了什么、破坏了什么。
9. 顺手在仓库里补一份 `CONTRIBUTING.md`，写清分支命名、提交信息规范、PR 检查项。

**验收标准**
- 上游仓库里能看到你发起的、已合并的 PR。
- 你的 Fork 的 main 与上游 main 一致（`git log` 或网页端对比无差异）。
- 存在一个 Release 页面，附有变更说明。

**提示**
- 同步 Fork 优先用 `rebase`，避免 `merge upstream/main` 产生一堆无意义的合并提交。
- 推送到被改写过历史的分支必须用 `--force-with-lease`，不要用裸 `--force`，以免覆盖别人的提交。
- 解决冲突时先读清楚 `<<<<<<<` / `=======` / `>>>>>>>` 两侧各是谁的改动，再决定保留哪一部分。

---

## 完成进度

- [ ] 练习 1：仓库、提交与 README
- [ ] 练习 2：分支、合并与 Pull Request
- [ ] 练习 3：Issue 驱动与 Projects 看板
- [ ] 练习 4：GitHub Actions 持续集成
- [ ] 练习 5：开源协作全流程
