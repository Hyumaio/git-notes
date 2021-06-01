## 一
##### 描述
之前在 github 上看的本地 commit 总是没有头像，后来发现是 `git config --local`(*\<project>/.git/config*) 没有 user 信息，所以使用了 `git config --global`(*~/.gitconfig*)。
##### 解决方案
在 `git config --global` 里设置经常使用的 user 信息，并在操作需要使用不同的 user 信息的 git 仓库时，**记得先 git config --local 设置新的 user 信息！！！**

## 二
##### 描述
commit 之后 push 失败，提示远端有未同步到本地的更新，需要先 fetch 再 merge，并提交一次关于此次 merge 的 commit，之后才能成功 push，这样会导致莫名其妙多出一个没有必要的 merge commit。尝试在 fetch 之前不执行 commit，但会提示需要先 commit 保存本地的修改，才能执行 fetch 操作，所以也一样会出现一个 merge commit。
##### 解决方案（大雾？）
看了一些文章思考过后发现，其实这样的 commit 是有意义且必要的，在最小化模型下，一次 merge 包含三个 commit，一个是远程拉取的对方提交的 commit，一次是自己在本地提交的 commit，一次是关于这两次提交的合并的 merge commit，前两次 commit 精确地记录了对方和我各修改了哪些代码，第三次 commit 则是为了在合并对方和我各自的提交之后，也就是整合代码，merge 结束之后，很显然我们要基于这次更改提交 commit (merge commit)，这是仅当存在 conflict 的情况下会发生的事情。当不存在 conflict 时，就不需要解决冲突，那么 merge 的时候 git 便不会提示你先 commit 本地修改之后再去 merge，所以三次 commit 就变为了两次，当你需要强调这次 merge 的时候，你可以使用 merge commit 的描述，最后 commit log 会类似于："others' commit msg --> merge commit msg"，当你需要强调自己的修改而不是这次 merge 操作时，你可以使用自己的 commit 描述，commit log 类似于："others' commit msg --> yours' commit msg"。（**注意：这是单分支上的 merge 操作，不是合并分支的 merge**，但其实合并分支的 merge 操作与其原理是差不多的，只是一些细节上的差异）

## 三
##### 描述
git rebase 第一次使用。
##### 解决方案
第一次提交 readme 的变动：
```bash
git checkout -b gcy_readme  # 新建分支
git add README.md  # 添加修改
git commit -m "README change"  # commit
```
然后发现之前创建新分支时使用的 master 已经很落后于现在 origin 的 master 了，这时需要重新拉取远端内容再做修改：
```bash
# on master
git fetch origin  # 拉取远端内容（默认master）
git rebase origin  # 把 master rebase 到 origin/master
git checkout gcy_readme  # 切换到需要 rebase 的分支上
# on gcy_readme
git rebase origin/master  # 把当前分支 rebase 到 origin/master HEAD
git log  # 查看 rebase 是否成功
git push --force  # rebase 后需要 force push
```

## 四
##### 描述
合并多个commit。
##### 解决方案
1). commands:
- `git rebase -i ~HEAD^3`: 从当前 commit 开始合并最后三个 commit; `-i`指开启 interactive editor
- `git rebase -i [startpoint] [endpoint]`: 从 startpoint 开始，合并到 endpoint（不包含 startpoint），endpoint 可不指定，默认值为当前分支 HEAD 所指向的 commit

2). interactive editor:
```
- p, pick: use commit
- r, reword: use commit, but edit the commit message
- e, edit: use commit, but stop for amending
- s, squash: use commit, but meld into previous commit
- f, fixup: like "squash", but discard this commit's log message
- x, exec: run command (the rest of the line) using shell
- d, drop: remove commit
```
- 从先提交的 commit 开始，依次往后列出所有需要合并的 commit
- <s>commit 可以重新排序，命令将会从上而下执行（例如合并两个 commit 时，将之后的 commit 放到最上面并 pick，对下一个 commit 执行 squash 的时候就可以合并到这个更新的 commit，并且 commit message 及 commit info 都会保持到这个更新的 commit）</s>（貌似会出现 conflict）
- use commit 的意思是使用这次 commit 进行编辑
- reword 只会要求更改 commit message, edit 将会在 rebase 到这一行时停下来，要求修改 commit message & commit contents，执行 `git commit --amend` 可以修改 message，执行 `git rebase --continue` 将会继续合并流程
- drop 或是删除掉一个 commit 行将会导致 commit 丢失，慎用

3). 修改 commit message

4). 如果 rebase 导致切换到了新的分支，创建一个新分支接收此次改动，switch 到原分支执行 `git rebase <new-branch>` 即可
