# Git Command Notes


## git branch ...
- `git branch -d <branch-name>`: 删除分支
- `git branch -d -r <remote-tracking-branch-name>`: 删除远程跟踪分支（类似 origin/xxx 的格式，它是一个只读的，用户不可自行改变的，记录了当前本地分支xxx所记录的远程分支xxx的更改的分支形式，当 fetch 时会自行创建or移动）


## git clone ...
- `git clone -b <remote-branch-name> <remote-url>` 会下载所有分支内容，在下载完成后帮你切换到 -b 指定的分支，如果只需要确定的单独一个分支，使用 --single-branch。
- `git clone <remote-url> <directory-name>` 会使用 directory-name 代替默认的名称（远程仓库的仓库名）。
- `--depth=n` 参数会截断 commit 个数为 n，并且会隐式地使用 --single-branch 参数，除非你显式地给定了 --no-single-branch 参数。


## git config ...
- `git config user.name/email`: 设置当前仓库 commit 所使用的信息，email 作为唯一识别 contribution 的标志（大小写不敏感），name 暂时不知道有什么用处
- `git config user.email`: 可以设置当前仓库 commit 所使用的邮箱，这个邮箱在 github 上可以是 primary/backup
- github 可以关联多个邮箱账号，只要你 commit 时使用的邮箱在 github 的关联邮箱里，系统就会把这次 commit 识别为一次有效的 contribution
- `git config --global core.quotepath false`, 可以使 git status 正确显示中文


## git pull ...
- `git pull` 相当于 `git fetch` + `git merge`。当有冲突时会让你解决冲突并就这一次 merge 提交新的 commit，会保留两端之前的 commit 并加上此次 merge 的 commit；当没有冲突时，pull 会实现快速 merge，这时会生成一个文件让你去填写这次 merge 的 commit，同样会保留两端之前的 commit，然后再加上此次 merge 的 commit


## git push ...
- `git push -u <remote-name> <branch-name>`: 把分支推到远程库并且关联
- `git push origin -d <remote-branch-name>`: 删除远程分支


## git remote ...
- `git remote add <remote-name> <remote-url>`: 添加远程仓库为 remote-name
- `git remote set-url --add <remote-name> <remote-new-url>`: 添加新远程仓库为 remote-name
- `git remote set-url --delete <remote-name> <remote-url>`: 删除名为 remote-name 的远程仓库

## git status ...
- `-s` 参数可以显示简短版的 git status


## Others
- 取消到已经 push 到远程的 commit：

	```
	git reset <commit-id> --hard, 注意 --hard 会撤销你当前工作区的改动，请先保存
	commit-id 是你想回到的版本 id
	
	git push --force 强制推送到远程仓库
	
	```
