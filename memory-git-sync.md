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

**关键约束**: 记忆目录名是按**那台机器上工程的绝对路径**生成的(把 `:` `\` 换成 `-`,如 `D:\OverSeasGame\...\Water` → `D--OverSeasGame-...-Water`)。各机路径**可以不同**——记忆 .md 内容与路径无关,只要 clone 进"那台机器对应的目录名"即可。

**新电脑恢复**: ①装 Claude Code + 登录一次(本体/凭证不随 git 走) ②工程 clone 到任意路径 ③先在该工程目录开一次 Claude Code,让它自动创建 `C:\Users\<用户名>\.claude\projects\<对应目录名>\`,查看实际生成的目录名 ④`git clone <记忆仓库>` 到那个目录的 `memory` 子目录。

**日常维护(我不会自动 push)**: 更新过记忆后,在 memory 目录 `git add -A && git commit && git push`;换电脑开工前 `git pull`。我每次只改 .md 文件,提交推送需用户(或被显式要求时由我)执行。

工程本体能否在新电脑用,见 [[art-replication-scope]] 同属该工程;Unity 版本/测试见 [[unity-batchmode-tests]]。
