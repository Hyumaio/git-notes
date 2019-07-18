# 踩坑记录


## 问题一

##### 描述
之前在 github 上看的本地 commit 总是没有头像，后来发现是 `git config --local`(*\<project>/.git/config*) 没有 user 信息，所以使用了 `git config --global`(*~/.gitconfig*)。

##### 解决方案
在 git config --global 里设置经常使用的 user 信息，并在操作需要使用不同的 user 信息的 git 仓库时，**记得先 git config --local 设置新的 user 信息！！！**

