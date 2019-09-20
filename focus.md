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
然后发现之前创建新分支时使用的 master 已经很落后于现在 origin 的 master了，这时需要重新拉取远端内容再做修改：
```bash
git fetch origin  # 拉取远端内容（默认master）
git rebase origin  # 添加修改
git checkout gcy_readme  # 切换到自己的=修改分支上
git rebase origin/master  # 把修改 rebase 到 master
git log  # 查看 rebase 是否成功
git push --force  # rebase 后需要 force push
```
