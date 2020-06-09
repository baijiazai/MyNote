# git 常用命令
## 新建代码库
- 在当前目录新建一个Git代码库
	- `$ git init` 

## 配置
- 显示当前的 git 配置
	- `$ git config --list`
- 编辑Git配置文件
	- `$ git config -e [--global]`
- 设置提交代码时的用户信息
	- `$ git config [--global] user.name "[name]"`
	- `$ git config [--global] user.email "[email]"`

## 增加/删除文件
- 添加指定文件到暂存区
	- `$ git add *`
	- `$ git add [file1] [file2] ...`
- 添加指定目录到暂存区，包括子目录
	- `$ git add [dir]`
- 添加当前目录的所有文件到暂存区
	- `$ git add .`
- 删除工作区文件，并且将这次删除放入暂存区
	- `$ git rm [file1] [file2] ...`
- 停止追踪指定文件，但该文件会保留在工作区
	- `$ git rm --cached [file]`
- 改名文件，并且将这个改名放入暂存区
	- `$ git mv [file-original] [file-renamed]`

## 代码提交
- 提交暂存区到仓库区
	- `$ git commit -m [message]`
- 提交暂存区的指定文件到仓库区
	- `$ git commit [file1] [file2] ... -m [message]`
- 提交工作区自上次commit之后的变化，直接到仓库区
	- `$ git commit -a`
- 提交时显示所有diff信息
	- `$ git commit -v`
- 使用一次新的commit，替代上一次提交
	- `$ git commit --amend -m [message]`
- 重做上一次commit，并包括指定文件的新变化
	- `$ git commit --amend [file1] [file2] ...`

## 查看信息
-  显示有变更的文件
	-  `$ git status`
-  显示当前分支的版本历史
	-  `$ git log`
-  显示commit历史，以及每次commit发生变更的文件
	-  `$ git log --stat`
-  搜索提交历史，根据关键词
	-  `$ git log -S [keyword]`
-  显示某个文件的版本历史，包括文件改名
	-  `$ git log --follow [file]`
- 显示过去5次提交
	- `$ git log -5 --pretty --oneline`
- 显示暂存区和工作区的差异
	- `$ git diff`
- 显示暂存区和上一个commit的差异
	- `$ git diff --cached [file]`

## 远程同步
- 下载远程仓库的所有变动
	- `$ git fetch [remote]`
- 显示所有远程仓库
	- `$ git remote -v`
- 显示某个远程仓库的信息
	- `$ git remote show [remote]`
- 增加一个新的远程仓库，并命名
	- `$ git remote add [shortname] [url]`
	- `$ git remote add origin [url]`
- 取回远程仓库的变化，并与本地分支合并
	- `$ git pull [remote] [branch]`
- 上传本地指定分支到远程仓库
	- `$ git push [remote] [branch]`
	- `$ git push origin master`
- 克隆仓库到一个新目录
	- `$ git clone [remote]`

## 撤销
- 恢复暂存区的指定文件到工作区
	- `$ git checkout [file]`
- 恢复某个commit的指定文件到暂存区和工作区
	- `$ git checkout [commit] [file]`
- 恢复暂存区的所有文件到工作区
	- `$ git checkout .`
- 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
	- `$ git reset [file]`
- 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
	- `$ git reset --hard [commit]`
- 重置当前HEAD为指定commit，但保持暂存区和工作区不变
	- `$ git reset --keep [commit]`
- 暂时将未提交的变化移除，稍后再移入
	- `$ git stash`
	-  `$ git stash pop`

## 其他
- 生成一个可供发布的压缩包
	- `$ git archive`