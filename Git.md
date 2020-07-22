### 新手学习 [Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
### [官网文档](https://git-scm.com/docs)
### 国外网友制作的GIT小抄：[Git Cheat Sheet](https://gitee.com/liaoxuefeng/learn-java/raw/master/teach/git-cheatsheet.pdf)

### 个人整理
1. git 是分布式版本控制系统

2. git理解  
	一：工作区、版本库(又名仓库)repository  
	二：版本库（隐藏目录.git）,主要暂存区(stage)、主分支(master)、指向master的HEAD指针  
	三：远程仓库  
	四：分支管理  
	五：标签管理  
3. github，参加开源项目，先在开源项目上点击"fork",就会在自己账号下克隆一个仓库，然后本地在克隆一份自己的远程仓库，最后如果想开源项目接收你的修改，则可以在github发起pull request，人家是否接受那可不一定哦！

4. 忽略特殊文件，工作区根目录创建 .gitignore 文件

5. 配置文件：当前用户./git/config ; 全局：用户主目录./gitconfig

6. 搭建git服务器，建裸仓库，即没有工作区; git init --bare <dirname\>

7. git图形化管理工具：Sourcetree、GitHub Desktop

8. 整理命令
	+ git init : 初次化版本库
	+ git add <file\> : 工作区文件添加到暂存区
	+ git branch : 查看分支
	+ git branch <name\> : 创建分支
	+ git branch -d <name\> : 删除分支
	+ git branch -D <name\> : 强行删除分支
	+ git branch --set-upstream <branch\> origin/branch : 建立本地分支和远程分支关联
	+ git commit -m 'desc' : 暂存区文件提交到分支
	+ git clone <url\> : 克隆远程仓库，URL支持包括https、ssh(快)
	+ git checkout -- <file\> : 文件本地库版本回退到分支最新版本
	+ git checkout <name\> 或 git switch <name\> : 切换分支
	+ git checkout -b <name\> 或 git switch -c <name\> : 创建+切换分支
	+ git checkout -b <branch\> origin/branch : 本地创建和远程分支对应的分支
	+ git cherry-pick <commit\> : 把提交的"复制"到当前分支
	+ git check-ignore -v <checkname\> : 检测文件为什么被忽略
	+ git config [--global] alias.<aliasname\> <name\> : 设置别名
	+ git diff <file\> : 对比文件修改内容
	+ git log --graph: 提交历史
	+ git log --graph --pretty=oneline --abbrev-commit
	+ git merge <name\> : 合并某个分支到当前分支	
	+ git merge --no-ff -m 'desc' <branch\> : 合并分支，策略使用Fast Forward
	+ git pull : 从远程仓库抓取分支
	+ git push -u <name\> master : 第一次推送master分支到远程仓库
	+ git push <name\> master : 推送master分支到远程仓库
	+ git push <origin\> <branch\> : 本地分支推送到远程仓库
	+ git push <origin\> <tagname\> : 推送本地标签到远程仓库
	+ git push <origin\> --tags : 推送全部未推送过的本地标签
	+ git push <origin\> :refs/tags/<tagname/> : 删除远程标签
	+ git reflog : 命令历史
	+ git rm <file\> : 暂存区删除文件命令
	+ git remote -v : 查看远程仓库信息
	+ git remote add <name\> <url\>
	+ git remote rm <origin\> : 删除
	+ git reset HEAD <file\> : 丢掉暂存区的文件
	+ git reset --hard <commit_id\> : 版本回退
	+ git rebase : 把本地未push的分叉提交历史整理成直线，方便查看历史提交
	+ git status : 版本库状态
	+ git stash : 储藏现场
	+ git stash list : 储藏的现场列表
	+ git stash apply <stash、>: 恢复现场，stash内容不删除
	+ git stash pop : 恢复现场，stash内容删除
	+ git tag : 查看所有标签
	+ git tag <tagname\> [commid_id]: 新建标签，默认为HEAD，可以指定提交commit_id
	+ git tag -a <tagname\> -m 'desc' [commid_id]: 指定标签并带信息
	+ git tag -d <tagname\> : 删除本地标签