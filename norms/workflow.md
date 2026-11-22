# 执行口径

> 依据：`pyai-meta-repo` 的 AGENTS C0–C10、`meta/rules/engineering.md`（E10 / E11 / E25）、`meta/skills/github`、`meta/skills/proxy`、`meta/skills/route`。
> 这里是操作层，规则本身不回抄；冲突时以 meta-repo 为准。

## 1. 接活

- [ ] 读 `meta/skills/route/SKILL.md` 匹配 playbook；匹配不到就停下问 owner，不硬套（C1）。
- [ ] 把 playbook 步骤拷成 todo，再动手；多步任务边做边更新。
- [ ] 先分清是「只查不改」还是「要改」——调查类不该动文件。
- [ ] 验证机制或试流程时，优先在已有仓库里做（PR / discussion 都可以）；确实需要新建试验仓库的，用完删除或转私有，并在 `log/` 留一行说明。

## 2. 验证

- [ ] 每个结论都要有命令输出或文件内容作证据；没有就写「未验证」，不写「应该没问题」（C0 / C4）。
- [ ] 负向用例：把输入改坏，确认检查真的报错。只跑通一次绿灯，说明不了检查有效。
- [ ] 报「完成」前逐项核对：哪些真做了、哪些没做、哪些只是说过了（C9）。
- [ ] 状态分级说清楚：已实现 / 已提交 / 已推送 / 已合并，不混用「完成」。

## 3. 提交与 PR

- [ ] 一个 commit 一件事，中文描述 + 英文类型前缀（E10）。
- [ ] PR 说明写清：改了什么、为什么、没改什么、证据、风险、未验证项。
- [ ] agent 的分支用 `git-agent` 推、PR 用 `gh-agent` 开；`gh` 只用于 owner 动作。
- [ ] 不自合并（C10）；合并方式按目标仓库：有集成分支的发布 PR 用 merge commit，其余 squash。
- [ ] 写操作前先确认身份：`gh` 是 owner、`gh-agent` 是 agent；涉及对方账号下的仓库时，先说清由谁执行，别假设谁能做。
- [ ] **本仓库例外**：内容由 agent 写，PR 用 owner 身份提交、由 agent 合并——owner 保留审阅入口和提交归属。其它仓库不适用这条。

## 4. 共享层改动

- [ ] `meta/rules`、`meta/playbooks`、`meta/skills` 是快照内容，改一个文件 5 个项目立刻全红。
- [ ] 顺序固定：合并 meta PR → 拿新提交号 → 同步 5 个项目（改文件 + 更新 `meta/SOURCE.md` + 各开 PR）。
- [ ] 同步 PR 必须指向**已合并**的提交，不能指向 PR 分支。

## 5. 环境

- [ ] GitHub 直连不稳（`Failed to connect` / `HTTP2 framing layer`）时用一次性代理参数，端口先探再试。
- [ ] 一次性产物（脚本、PR 草稿、备份、临时 worktree）放工作区 `TEMP/`，收尾清空，不写 `/tmp`（E25）。
- [ ] 长命令写脚本文件再执行；内联堆引号容易被解析器打断。

## 6. 收工

- [ ] 项目校验与 GitHub 策略校验都绿：`check-project-contract`、`check-github-policy`。
- [ ] 分支只剩 main、工作区干净、`TEMP/` 只剩 `.gitkeep`。
- [ ] `log/` 补一节这次的决策与遗留；新踩的坑进 `log/learnings.md`。
- [ ] 只记有复用价值的决策与教训；仓库自身的维护流水（改名、重建、搬家）不记。
