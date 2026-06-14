---
name: memory-git-sync
description: 记忆通过私有 git 仓库跨电脑同步的方法 + push/pull 维护流程
metadata: 
  node_type: memory
  type: reference
  originSessionId: 7ecac03b-2567-42fd-a084-46ec9c45fcc0
---

记忆目录已设为独立私有 git 仓库,用于跨电脑同步(用户多台电脑开发同一工程)。

**记忆仓库**: https://github.com/mayuwei0623-web/Claude.git (PRIVATE, branch main)
**本机记忆目录(= 该仓库工作区)**: `C:\Users\<用户名>\.claude\projects\D--OverSeasGame-GameTestProject-WaterSort-Water\memory\`

**关键约束**: 记忆目录名 `D--OverSeasGame-...` 是按工程绝对路径生成的。新电脑必须把工程 clone 到完全相同的绝对路径 `D:\OverSeasGame\GameTestProject\WaterSort\Water`,记忆目录名才对得上,我才能自动读到。

**新电脑恢复**: ①装 Claude Code + 登录一次(本体/凭证不随 git 走) ②工程 clone 到上述同一路径 ③`git clone <记忆仓库>` 到该机实际的 `.claude\projects\...\memory`。

**日常维护(我不会自动 push)**: 更新过记忆后,在 memory 目录 `git add -A && git commit && git push`;换电脑开工前 `git pull`。我每次只改 .md 文件,提交推送需用户(或被显式要求时由我)执行。

工程本体能否在新电脑用,见 [[art-replication-scope]] 同属该工程;Unity 版本/测试见 [[unity-batchmode-tests]]。
