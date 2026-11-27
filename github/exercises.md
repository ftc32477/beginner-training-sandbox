# GitHub 练习清单

共 5 条练习，按由易到难排列。建议按顺序完成，每条练习都给出**目标**、**操作步骤**和**验收标准**。

> 约定：所有练习都在你自己 fork 出来的练习仓库里完成，不要直接向主仓库推送。

---

## 练习 1：第一个仓库与第一次提交（入门）

**目标**：熟悉仓库创建、克隆、暂存区与提交。

**步骤**
1. 在 GitHub 上新建一个空仓库 `git-practice`，初始化时勾选生成 `README.md`。
2. 本地执行 `git clone <仓库地址>`，进入目录。
3. 新建文件 `hello.txt`，写入一行任意内容。
4. 依次执行 `git status` → `git add hello.txt` → `git status`，观察状态变化。
5. 提交：`git commit -m "docs: add hello.txt"`。
6. 推送：`git push origin main`。

**验收标准**
- GitHub 网页上能看到 `hello.txt` 以及对应的提交信息。
- 能说清工作区、暂存区、本地仓库、远程仓库四者的区别。

---

## 练习 2：分支、合并与提交历史（基础）

**目标**：掌握分支的创建、切换与合并，理解 fast-forward 与三方合并。

**步骤**
1. 从 `main` 创建分支：`git switch -c feature/greeting`。
2. 在 `hello.txt` 末尾追加一行，提交一次。
3. 切回 `main`，在 `hello.txt` 不同位置追加一行，提交一次（制造分叉）。
4. 执行 `git merge feature/greeting`，观察合并结果。
5. 用 `git log --oneline --graph --all` 查看提交图。
6. 删除已合并分支：`git branch -d feature/greeting`。

**验收标准**
- `git log --graph` 中能看到明显的分叉与合并点。
- 能解释本次是 fast-forward 还是三方合并，以及原因。

---

## 练习 3：Issue + Pull Request 协作闭环（进阶）

**目标**：走通「建 Issue → 开分支 → 提 PR → 评审 → 合并」的完整协作流程。

**步骤**
1. 在仓库中新建一个 Issue，标题为 `补充项目使用说明`，正文写清期望补充的内容。
2. 从 `main` 拉出分支 `docs/usage`，在 `README.md` 中新增「使用方法」小节，提交时在信息里引用 Issue：`docs: add usage section (closes #1)`。
3. 推送分支并在网页上发起 Pull Request，描述中链接该 Issue。
4. 请至少一位同学（或自己开小号）在 PR 中留下一条行内评论，提出一处修改意见。
5. 根据意见追加提交，然后在 PR 中回复该评论。
6. 使用 **Squash and merge** 合并 PR，确认 Issue 被自动关闭。

**验收标准**
- Issue 因关键字 `closes #1` 在合并后自动关闭。
- PR 页面能完整看到「评论 → 修改 → 回复 → 合并」的记录。

---

## 练习 4：冲突解决、Rebase 与 Cherry-pick（高级）

**目标**：在复杂历史下保持提交线整洁，并把单个提交精确搬运到其他分支。

**步骤**
1. 创建 `feature/a` 与 `feature/b` 两个分支，都修改 `hello.txt` 的同一行并各自提交。
2. 先合并 `feature/a` 到 `main`，再合并 `feature/b`，触发冲突。
3. 手动解决冲突（保留两部分合理内容），`git add` 后完成合并，并解释 `<<<<<<<` / `=======` / `>>>>>>>` 的含义。
4. 另建分支 `feature/c`，在其中做两次提交，用 `git rebase -i main` 把两次提交压缩为一次。
5. 在 `feature/c` 上用 `git log` 找到目标提交的哈希，执行 `git cherry-pick <hash>` 到 `main`。
6. 若 rebase 出错，使用 `git rebase --abort` 或 `git reflog` 恢复现场。

**验收标准**
- `main` 上无冲突残留标记，历史线性可读。
- 能说明 `merge`、`rebase`、`cherry-pick` 三者适用场景的差异，以及为什么不要对已推送的公共分支做 rebase。

---

## 练习 5：分支保护 + Actions 自动化发布（综合）

**目标**：用仓库规则与 CI 把协作规范固化下来，实现「测试通过才能合并」并产出一次 Release。

**步骤**
1. 在仓库 `Settings → Branches` 为 `main` 添加保护规则：要求 PR、要求至少 1 个审批、要求状态检查通过。
2. 在 `.github/workflows/ci.yml` 中添加一个工作流：在 `pull_request` 与 `push` 到 `main` 时触发，运行一个最小校验（例如 `echo` 或 `npm test` / `pytest`）。
3. 验证：直接向 `main` 推送应被拒绝；PR 未通过检查时合并按钮不可用；修复后检查变绿方可合并。
4. 使用语义化标签打版本：`git tag -a v1.0.0 -m "first release"` 并 `git push origin v1.0.0`。
5. 再添加一个工作流，在推送 `v*` 标签时自动创建 GitHub Release，并把构建产物作为附件上传。
6. 在 Release 说明中整理本次版本的变更清单（可从已合并的 PR 汇总）。

**验收标准**
- 一次不合规的推送被分支保护拦下并留有截图或记录。
- 打标签后 Actions 自动运行并成功生成 Release 页面与附件。
- 能说明 R 为什么发布流程要依赖 CI 而不是人工判断。

---

## 进度自查表

| # | 练习 | 难度 | 关键技能 | 完成 |
|---|------|------|----------|------|
| 1 | 第一个仓库与第一次提交 | ★ | clone / add / commit / push | ☐ |
| 2 | 分支、合并与提交历史 | ★★ | branch / merge / log --graph | ☐ |
| 3 | Issue + PR 协作闭环 | ★★★ | Issue / PR / Review / Squash | ☐ |
| 4 | 冲突、Rebase 与 Cherry-pick | ★★★★ | 冲突解决 / rebase -i / cherry-pick | ☐ |
| 5 | 分支保护 + Actions 发布 | ★★★★★ | 分支保护 / Actions / Tag / Release | ☐ |
