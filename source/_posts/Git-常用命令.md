title: Git 常用命令（更新中）
date: 2015-05-25 17:18:32
tags: Git
categories: 学习记录
---

<blockquote class="blockquote-center">假如全世界的工具你只能保留一个在电脑，你的选择是什么？Git.
  --《我的老大是屌丝》摘
</blockquote>

从设计转型开发的第一天起，老大只教我一件事，使用 Git。
比起一上来就给我各种任务写 HTML/CSS/JS 调各种 Bug, 让我熟练掌握 Git 这件事至今我都非常感激。
毫无疑问地说，Git 是当今编程学习里最基本的必备技能。

Git 的强大一本书都不足以全部说明，更何况一篇博客。
本文记录了我 3 年来使用 Git 最频繁的命令（不包括最基本的add/commit/push/pull等），
很负责地说，学会这些基本也就能快乐地玩转 Git 了 **（持续整理更新中）**。

Hope you enjoy!

<!-- more -->

## 1. 超实用 Alias

```bash
alias g="git"
alias gb="git branch"
alias gco="git checkout"
alias gcmsg="git commit -m"
alias gamend="git commit --amend -C HEAD"
alias gst="git status"
alias log="git log --oneline --graph --decorate --color=always"
alias logg="git log --graph --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(bold white)—     %an%C(reset)%C(bold yellow)%d%C(reset)' --abbrev-commit --date=relative"
```

## 2. 取回远端 `master` 与本地 `master` 分支合并

```bash
gco master

g fetch --all 或者
g fetch origin master

g reset --hard origin/master(本地没有修改，所以完全覆盖也没关系) 或者
g rebase origin/master（本地有修改还没push）
```

## 3. 推送分支到远端

假设现在所在的分支是`import`，指定推送到远端分支`liujin-import`
```bash
g push origin import:liujin-import
```

假如远端的 `liujin-import` 分支已经不需要，可以直接覆盖掉
```bash
g push -f origin import:liujin-import
```

## 4. 追加文件到某个 commit

有时候修完某功能并提交了 commit 之后才发现还有一点小修改，这时候又不想再提交一个commit，可以追加这个文件到前一个commit，步骤如下：

```bash
git add 你要追加修改的文件
git commit --amend -C HEAD 或者 gamend
```

## 5. 查找包含某文件的 commit

```
git log 文件路径
git show commit_id
```

或者

```
git log --follow filename(绝对路径)
```
Ref: [List all commit for a specific file](http://stackoverflow.com/questions/3701404/list-all-commits-for-a-specific-file)

## 6. 把一个 commit 分拆为两个 commit

老大常说要养成一个小改动对应一个commit的习惯，但是有时候写得太乱懒得去分割就把很多改动做成了一个commit，这样子增加了以后维护的难度，所以要把一个 commit 分拆为多个 commit 怎么办呢？

- 首先把你要分拆的 file reset 了：

```
git reset HEAD~1 path/to/file
# This doesn't delete your changes to path/to/file
```

- 接着修改当前这个 commit 的 message，命令是：

```
git commit --amend -v
# -v参数是打开editor编辑
```

- 然后就可以把 reset 出来那个 file 新建一个 commit，命令是：

```
git commit -v path/to/file
```

这样就把一个 commit 分拆为两个啦，^_^

## 7. 删除某些 commit

```
git rebase -i HEAD~10
```

## 8. 追加修改到之前某个 commit

假如 `gst` 发现已经有文件被修改，这时候需要把修改暂存起来。

```
git stash
```

接着找到你需要追加修改的那个commit id，如`4b739bb`

```
g rebase 4b739bb~ -i 或者
g rebase -i HEAD~5 #列出最近5个commit
```

这时候会自动打开编辑器，把你需要修改的 commit 前面的 `pick` 改成 `edit`，保存，关闭编辑器，这时候会回到终端，再输入:

```
g stash pop
```

把暂存的修改读出来，然后做修改，`g add .`，最后

```
g rebase --continue
```

## 9. 查找含有特定关键字的 commit

- `git log --grep`
  最基本的用法
- `git log --grep=frotz --grep=nitfol --since=1.month`
  查找一个月以内commit log message里含有 `frotz` 或者 `nitfol` 的 commits

- `git log --grep=frotz --author=Linus`
  查找指定作者

- `git grep -l -e frotz --and -e nitfol`
  查找同一行含有 `frotz` 和 `nitfol` 的文件

- `git grep -l --all-match -e frotz -e nitfol`
  查找文件里面含有 `frotz` 和 `nitfol` 的文件（不局限于同一行）

## 10. 清空 git working copy 还没追踪的文件

- `git clean -f`

- `git clean -f -d`
  如果还想删除目录

- `git clean -f -X`
  如果只是想删除忽略的文件

- `git clean -f -x`
  如果想删除忽略和非忽略的文件

## 11. 清理本地仓库

长时间做一个项目，经常需要 `git fetch`，这样做每次都会拉回远端的全部分支。
即使远端有些分支已经删除，但是运行`git branch -a`还是会显示已删除的分支，
长时间下来这个列表就会很长很长，这时候就需要清理一下本地的仓库了：

```bash
git remote prune origin
# `prune`会删除任何不存在于远端仓库的分支，这样运行 `git branch -a`命令的时候顿时就清静了
```

## 12. 创建追踪远端分支的本地分支

```bash
gb dev origin/r1-dev
#Branch dev set up to track remote branch r1-dev from origin.
```

## 13. 分支移动

```bash
g branch -f master HEAD~3
# 把 master 分支强制移到 HEAD 前面第三个 commit
```

## 14. Revert一个 Merge

`git revert -m 1 M` -> W

 ```
 ---o---o---o---M---x---x---W
               /
       ---A---B
 ```

## 15. 获取短的 commit hash

```bash
git rev-parse --short HEAD
```

## 16. commit 了以后才记起来忘了创建 `.gitignore`, 垃圾文件都已经提交

比如说一个nodejs项目，一开始的时候就忘记了创建`.gitnore`文件忽略掉`node_modules`的内容，所以其中的内容就已经被提交了。

即使紧接着你在`.gitignore`加了一行`node_modules`, 已经被提交的文件是不会自动删除的。

这时候你就需要做的就是：

```bash
git rm --cached path/to/ignored
#nodejs案例就是
git rm -r --cached node_modules
```

## 17. 提交所有被删除的文件

```
$ git add -u
```
这个命令告诉 git 自动更新已跟踪的文件, 包括被删除的已跟踪文件。

如果你用的是 git 2.0

```
$ git add -u :/
```

友情提示：从 git 2.0(2013年中)开始，以上命令会 stage 整个 working tree 的文件。
如果你只是想 stage 当前目录的文件，应该这么用：

```
$ git add -u .
```
详情可以去搜 "“git add -A” 和 “git add .” 的区别".

Ref: [StackOverflow](http://stackoverflow.com/questions/1402776/how-do-i-commit-all-deleted-files-in-git)

## 18. 撤销上一次 `git add .` 操作

这种情况通常是因为忘记添加`.gitignore`文件，或者一时手快把一些非必要的文件（如`node_modules`)跟踪了, 解决办法：

```bash
git reset
```
该命令会 unstage 你上一个 commit 增加的所有文件。

如果你只想 unstage 某些文件:

```
git reset -- <file 1> <file 2> <file n>
```

还可以 unstage 文件里某处的改动：

```
git reset -p
```

## 19. 值得一读
[Working with submodules](https://github.com/blog/2104-working-with-submodules)
[Managing large files with Git LFS](https://github.com/blog/2079-managing-large-files-with-git-lfs)
[`git bisect`](https://github.com/blog/2094-new-year-new-git-release)
