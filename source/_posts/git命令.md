---
title: git命令
date: 2023-09-26 15:47:10
description: 目前收录-stash/reset/cherry-pick/revert/reflog几个指令
tags:
  - git
categories:
  - git
---

```git
# 保存当前未commit的代码
git stash

# 保存当前未commit的代码并添加备注
git stash save "备注的内容"

# 列出stash的所有记录
git stash list

stash@{0}: WIP on ...
stash@{1}: WIP on ...
stash@{2}: On ...

# 删除stash的所有记录
git stash clear

# 应用最近一次的stash
git stash apply

# 应用最近一次的stash，随后删除该记录
git stash pop

# 应用第二条记录
git stash apply stash@{1}

# 删除最近的一次stash
git stash drop

# 恢复最近一次 commit
git reset --soft HEAD^

# reset --soft 相当于后悔药，给你重新改过的机会。对于上面的场景，就可以再次修改重新提交，保持干净的 commit 记录。
# 以上说的是还未 push 的commit。对于已经 push 的 commit，也可以使用该命令，不过再次 push 时，由于远程分支和本地分支有差异，需要强制推送 git push -f 来覆盖被 reset 的 commit。
# 还有一点需要注意，在 reset --soft 指定 commit 号时，会将该 commit 到最近一次 commit 的所有修改内容全部恢复，而不是只针对该 commit。


# 将某个分支的 一个或多个commit 复制到当前分支
git cherry-pick commit1 commit2
git cherry-pick commit1^..commit2
# 如中间有冲突，需解决重新提交到暂存区，然后运行
git cherry-pick --continue
# 放弃 cherry-pick,回到操作前的样子
git cherry-pick --abort
# 退出 cherry-pick,不回到操作前的样子
git cherry-pick --quit

# 将现有的提交还原，恢复提交的内容，并生成一条还原记录。
git revert 21dcdksjfhedjkaofadf
# 记录了所有的 commit 操作记录，便于错误操作后找回记录。
git reflog
```
