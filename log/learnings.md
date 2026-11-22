# 踩坑清单

## 公开与私密

- **公开前要查历史，不是查当前文件**：改名、删行只能处理最新版本；旧提交和 PR 分支里的原文仍可被翻到。要么重写历史，要么重建仓库。

## 校验脚本

- **`.DS_Store` 会让契约检查误报**：脚本用 `readdirSync` 比较文件清单，不经过 git，`.gitignore` 挡不住；报错还是「missing」，很难联想。修法两层——脚本遍历跳过 + 仓库 `.gitignore` 收录，本机另配全局 gitignore。
- **共享层一改，5 个项目立刻全红**：`meta/rules`、`meta/playbooks` 属于快照内容。必须先合并 meta PR、拿到新提交号，再同步 5 个项目（`meta/SOURCE.md` 要指向已合并的提交）。

## 环境

- **GitHub 直连不稳**：会出现 `Failed to connect … port 443` 或 `HTTP2 framing layer`。用一次性代理参数，端口先探（7897 / 7890 都见过），别凭上次的端口盲试。
- **GraphQL 字段名不能猜**：`isSponsorable`、`hasSponsorsListing` 不在 `RepositoryOwner` 上，必须用 inline fragment 落到 `User` / `Organization`。

## 规则与配置

- **免费计划的私有仓库**：rulesets、分支保护、tag 保护、secret scanning、CodeQL、私有漏洞报告全部 403，只有 Dependabot alerts 可用。策略里记「不可用」并做反向断言，不要静默跳过。
- **一次性产物不要放 `/tmp`**：整机共享，容易和其他会话的文件混在一起，也追溯不到来源。放工作区 `TEMP/`，用完清空（已立为 E25）。

## 工具

- 长 shell 命令里的嵌套引号容易被打断：写成脚本文件再 `sh` 它，比内联一长串稳。
- **GraphQL 传多行文本要用变量**：把带换行的内容直接写进 query 字符串会报 `Expected string or block string, but it was malformed`；改成 `-f body="$BODY"` 由变量传入即可。

## GitHub 账号与权限

- **成就与贡献按「动作」归属，不按仓库归属**：你在别人仓库里做的事照样记在你名下。
- **`gh` 与 `gh-agent` 的 token scope 不同**，能力边界也不同：动手前先看清楚，别假设对方能做。
- **token 没有 `delete_repo` 权限就删不了仓库**：`DELETE /repos/{owner}/{repo}` 返回 403「Must have admin rights」。替代做法是旧仓库改名留档、新建同名仓库；真要删只能网页操作。
