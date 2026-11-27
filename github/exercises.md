# GitHub 练习项（由易到难）

共 5 条练习，建议按顺序完成。每条包含目标、步骤与验收标准。

- [ ] 练习 1：创建仓库与第一次提交
- [ ] 练习 2：分支、修改与 Pull Request
- [ ] 练习 3：Issue 驱动开发与提交信息规范
- [ ] 练习 4：解决合并冲突与 Rebase
- [ ] 练习 5：GitHub Actions 自动化流水线

---

## 练习 1：创建仓库与第一次提交（入门）

**目标**：熟悉「克隆 → 修改 → 暂存 → 提交 → 推送」这条最基本的链路。

**步骤**
1. 在 GitHub 上创建仓库 hello-github，初始化时勾选生成 README.md。
2. 本地克隆：git clone git@github.com:<你的用户名>/hello-github.git
3. 新建文件 notes.md，写入三行你今天学到的 Git 命令。
4. 查看状态并暂存：git status，然后 git add notes.md
5. 提交并推送：git commit -m "docs: add git notes"，然后 git push

**验收标准**
- [ ] GitHub 网页上能看到 notes.md，且提交作者是你本人。
- [ ] 能用自己的话说明工作区、暂存区、本地仓库、远程仓库的区别。

---

## 练习 2：分支、修改与 Pull Request（初级）

**目标**：掌握功能分支工作流，完整走通一次 Pull Request。

**步骤**
1. 从 main 拉出分支：git switch -c feat/add-greeting
2. 在 README.md 末尾追加一段「问候语」章节并提交。
3. 推送分支：git push -u origin feat/add-greeting
4. 在网页上发起 Pull Request，描述中说明「改了什么、为什么改」。
5. 自己作为 reviewer 留一条评论，然后用 Squash merge 合并。
6. 回到本地清理：git switch main，git pull，git branch -d feat/add-greeting

**验收标准**
- [ ] PR 页面上能看到提交记录、评论与合并记录。
- [ ] 本地功能分支已删除，main 与远程保持一致。

---

## 练习 3：Issue 驱动开发与提交信息规范（中级）

**目标**：学会用 Issue 描述需求，并用可追溯的提交信息关联它。

**步骤**
1. 新建 Issue，标题为「README.md 缺少使用说明」，正文写出验收要点清单。
2. 记录 Issue 编号（例如 #7）。
3. 新建分支 docs/usage-guide，补充「使用说明」章节。
4. 用 Conventional Commits 规范提交，并在正文中关闭 Issue：
   git commit -m "docs(readme): add usage guide" -m "Closes #7"
5. 发起 PR 并合并，确认 Issue 自动变为 Closed。
6. 给该 Issue 添加 documentation 标签并指派给自己。

**验收标准**
- [ ] Issue 因 PR 合并而自动关闭。
- [ ] 提交信息符合 type(scope): subject 格式，正文说明了动机。

---

## 练习 4：解决合并冲突与 Rebase（中高级）

**目标**：理解冲突产生的原因，并区分 merge 与 rebase 对历史的影响。

**步骤**
1. 从 main 创建两个分支 feat/a 与 feat/b，让它们修改 README.md 的同一行。
2. 先合并 feat/a 到 main，再合并 feat/b，人为制造冲突。
3. 本地切到 feat/b 执行 git rebase main，手动解决冲突后 git add . 与 git rebase --continue。
4. 用 git log --graph --oneline --all 观察 merge 与 rebase 两种历史形态的差异。
5. 把观察结论追加到 notes.md 并提交。

**验收标准**
- [ ] 冲突被正确解决，两个分支的改动都保留了下来。
- [ ] 能说明为什么 rebase 会重写提交历史，因此不应对已共享的分支使用。

---

## 练习 5：GitHub Actions 自动化流水线（高级）

**目标**：为仓库搭建一条自动校验流水线，体验 CI 的门禁作用。

**步骤**
1. 在仓库根目录新建 .github/workflows/ci.yml。
2. 触发条件配置为 push 到 main 以及所有 pull_request。
3. 编写 job：检出代码 → 安装 Node → 执行 npm ci 与 npm test（无测试时可用 echo 占位）。
4. 提交并推送，在 Actions 页面观察运行结果。
5. 故意让一条检查失败，确认 PR 上出现红色状态；修复后确认变绿。
6. 在仓库设置中开启分支保护，要求 CI 通过才能合并到 main。

**验收标准**
- [ ] Actions 页面能看到成功与失败两种运行记录。
- [ ] CI 未通过时，PR 的合并按钮被禁用。

---

## 完成后的自检

- [ ] 5 条练习全部勾选完成。
- [ ] git log 中能看到一条清晰、可读的提交历史。
- [ ] 能完整讲清一次「从 Issue 到合并」的协作流程。
