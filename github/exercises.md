# GitHub 练习清单

共 5 条练习，按难度从易到难排列，建议顺序完成。

- [ ] 练习 1：Fork 与 Star（★ 入门，约 10 分钟）
- [ ] 练习 2：分支 + 提交 + 第一个 Pull Request（★★ 基础，约 20 分钟）
- [ ] 练习 3：用 Issue 管理任务并让提交自动关闭 Issue（★★★ 进阶，约 30 分钟）
- [ ] 练习 4：解决合并冲突并整理提交历史（★★★★ 熟练，约 45 分钟）
- [ ] 练习 5：GitHub Actions 自动化 + 分支保护 + 发布 Release（★★★★★ 综合，约 90 分钟）

---

## 练习 1：Fork 与 Star

**目标**：熟悉 GitHub 网页端基本操作，理解上游仓库（upstream）与派生仓库（fork）的关系。

**步骤**
1. 在 GitHub 上找一个开源仓库，例如 octocat/Hello-World。
2. 点击右上角 Star，把仓库加入收藏。
3. 点击 Fork，把它复制到你自己的账号下。
4. 在你的 fork 中，用网页编辑器修改 README.md，在末尾加一行：Forked by <你的用户名>。
5. 提交时写清 commit message，例如 docs: add fork note to README。
6. 记录你的 fork 页面地址。

**验收标准**
- 你的账号下存在该仓库的 fork。
- fork 的 README.md 中能看到你添加的那一行。
- 提交历史里存在你写的 commit message。

**提示**：fork 不会自动同步上游更新，之后可用网页上的 Sync fork 按钮拉取上游改动。

---

## 练习 2：分支 + 提交 + 第一个 Pull Request

**目标**：走通「开分支 → 改内容 → 提交 → 提 PR → 合并」的完整协作流程。

**步骤**
1. 克隆你的 fork 到本地：git clone <你的仓库地址>。
2. 从 main 创建新分支：git switch -c feat/add-exercises。
3. 在本文件中新增一条属于你自己的练习，需包含目标与验收标准。
4. 分 2~3 次提交，每次只做一件小事，提交信息使用约定式格式，例如 feat: add my own exercise、docs: polish exercise wording。
5. 推送分支：git push -u origin feat/add-exercises。
6. 在网页上点击 Compare & pull request，向 main 发起 PR。
7. PR 描述写清三点：改了什么、为什么改、如何验证。
8. 自己 review 一遍 diff，然后用 Squash and merge 合并。
9. 本地切回 main 并同步：git switch main && git pull，再删除已合并的本地分支。

**验收标准**
- PR 有标题和结构化描述。
- PR 的 diff 只包含本次练习相关改动。
- 合并后 main 上有你的新练习，本地 main 与远程一致。

**提示**：提交前用 git status 和 git diff --staged 确认没有误提交无关文件。

---

## 练习 3：用 Issue 管理任务并让提交自动关闭 Issue

**目标**：学会用 Issue 拆解任务，并用关键字把提交与 Issue 关联起来。

**步骤**
1. 新建一个 Issue，标题为「为每条练习补一句话简介」，正文写清背景与期望结果。
2. 给 Issue 打标签，例如 documentation、good first issue。
3. 创建分支 fix/issue-<编号>-summary。
4. 完成修改，提交信息中使用关闭关键字，例如 docs: add one-line summary (closes #<编号>)。
5. 发起 PR，在描述中写 Closes #<编号>，观察 Issue 与 PR 的双向链接。
6. 合并后回到 Issue 页面，确认它已被自动关闭。

**验收标准**
- Issue 有明确的标题、正文与标签。
- PR 页面右侧显示关联的 Issue。
- 合并后 Issue 状态为 Closed，并出现引用提交的链接。

**提示**：常用关闭关键字有 close、closes、fix、fixes、resolve、resolves，必须写在提交信息或 PR 描述里，写在代码注释里无效。

---

## 练习 4：解决合并冲突并整理提交历史

**目标**：掌握冲突产生的条件与解决方式，理解 merge 与 rebase 的差别。

**步骤**
1. 从 main 切出分支 a，修改本文件中同一行内容并提交。
2. 切回 main，修改同一行内容并提交，制造冲突。
3. 在分支 a 上执行 git rebase main，观察冲突标记 <<<<<<<、=======、>>>>>>>。
4. 手工解决冲突：保留正确内容、删除冲突标记，然后 git add <文件> 与 git rebase --continue。
5. 练习一次撤销：解冲突过程中执行 git rebase --abort，确认能干净回退，再重新做一遍。
6. 用交互式 rebase 整理历史：git rebase -i main，把 3 个零碎提交 squash 成 1 个。
7. 推送分支（已推送过则用 git push --force-with-lease），发起 PR 并合并。

**验收标准**
- 能说清什么情况下会产生冲突。
- 合并后文件中无冲突标记残留，内容符合预期。
- PR 中该分支只有 1 个干净提交。

**提示**：解冲突时先看 git status 中标记为 both modified 的文件；--force-with-lease 比 --force 安全，别人推过新提交时它会拒绝覆盖。

---

## 练习 5：GitHub Actions 自动化 + 分支保护 + 发布 Release

**目标**：把仓库升级为有 CI 检查、有保护规则、有版本发布的规范项目。

**步骤**
1. 写工作流：在 .github/workflows/ci.yml 中定义在 push 与 pull_request 时触发的 workflow，至少包含两个 job：
   - lint：检查 Markdown 或代码风格，例如 markdownlint-cli2、eslint、ruff。
   - test：运行项目测试，没有测试的项目写一个最小脚本并按结果 exit 0 或 exit 1。
   要求：设置 permissions: contents: read，使用 actions/checkout@v4，并为 job 设置 timeout-minutes。
2. 做缓存：为依赖安装步骤加上 actions/cache 或 setup action 自带缓存，让第二次运行明显变快。
3. 开分支保护：Settings → Branches 中为 main 添加规则——禁止直接推送必须走 PR、必须通过 CI 检查才能合并、至少 1 人 approve。
4. 验证规则：尝试直接向 main 推送确认被拒绝；再提一个会让 CI 失败的 PR，确认合并按钮变灰。
5. 加 CODEOWNERS：创建 .github/CODEOWNERS，为目录指定 owner，观察 PR 自动请求 review。
6. 发布 Release：打标签 git tag -a v0.1.0 -m "first release" 并推送 git push origin v0.1.0，在网页上基于该标签创建 Release，写清更新说明。

**验收标准**
- CI 在 push 与 PR 上都能触发，第二次运行因缓存而更快。
- 直接推送 main 被拒绝，CI 未通过的 PR 无法合并。
- 仓库存在 v0.1.0 标签与对应 Release 页面。

**提示**：写 Actions 时先在本地用 act 或直接在分支上试跑，避免反复污染 main；Release 说明建议按新增、修复、破坏性变更分组。
