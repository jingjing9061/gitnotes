
Git 版本管理工具
一 创建版本库
mkdir learngit //创建本地版本库
cd learntgit
pwd //查看当前目录
git init //把这个目录变成git可以管理的仓库
ls -ah //查看隐藏的文件
git add <file> //把文件添加到仓库  //把工作区修改的文件放到暂存区
git commit -m <message> //把文件提交到仓库 -m本次提交的说明 //把所有暂存区的内容提交到分支
二 版本回退
git log //查看提交历史
git log —-pretty=oneline //简洁版的历史记录
git reflog //查看命令历史 以便确定要回到那个版本
git reset --hard HEAD^ //回退到上一个版本 //HEAD^^上上版本 
git reset --hard 1094a //回退相对应的版本

git status //查看状态
//查看工作区和版本库里面最新版本的区别
git diff HEAD -- readme.txt

三 撤销修改
git checkout -- file //文件在工作区的修改全部撤销
分两种： 1 add前 撤销后回到和版本库一样的状态 
             2 add 后 再次修改 撤销后回到暂存区状态

四 删除文件
rm file //删除工作区的文件
git rm file   git commit file  //删除版本库的文件
git checkout — file //删除错了 版本库有还有可恢复  只能恢复commit 之前的文件
如版本库删除可以指定回到那个版本可恢复 $ git reset --hard 指定版本号

五 远程仓库	 
创建SSH Key
ssh-keygen -t rsa -C "1179012421@qq.com"
cd ~ //返回用户主目录
//查看隐藏的文件
ls -la
Cat id_rsa.pub

	仧.	 添加远程仓库
1   创建新的仓库 create a new repository  learngit
2  在本地 learngit仓库下 运行git remote add origin git@github.com:jingjing9061/learngit.git
3 git push -u origin master //把本地库所有的内容推送到远程库中   初次推送加-u 
    之后推送使用 git push origin master则可
	仧.	从远程库克隆
 git clone git@github.com:jingjing9061/learnspace.git //克隆一个本地库

六 分支管理
git checkout -b dev  //-b 创建并切换分支
等同于：
git branch dev
git checkout dev

git branch 查看当前分支      // *当前分支
git checkout master //切换回master分支
git merge dev //dev 分支的成果合并到master分支上
git branch -d dev //删除dev 分支
git branch -D <name>//强行删除
git log --graph --pretty=oneline --abbrev-commit //查看分支的合并情况

分支策略管理
git checkout -b dev 
git add readme.text
git commit -m ‘add merge’
git checkout master

（     --no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，
       而默认fast forward合并就看不出来曾经做过合并
    --squash：使用squash方式合并，把多次分支commit历史压缩为一次）	
git merge --no-ff  -m ’merge with --no-of’ dev   // -m 因为合并创建新的commit 所以加上m参数 把commit的参数描述加进去  



Bug 分支

bug 都可以通过临时分支修复 修复后合并分支 然后将临时分支删除

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

git stash //把现场工作储藏起来 恢复现场后继续工作 
git stash list//查看刚才隐藏的工作现场
git stash apply //恢复工作 不删除stash内容
git stash drop //仅删除stash 内容
git stash pop//恢复同时 删除stash内容

Feature分支
每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支
git branch -D <name> //丢弃一个没有被合并过的分支

多人协作

mster 分支是主分支，要时刻与远程同步
dev 是开发分支 团队成员都需要在上面工作，所以也需要与远程同步
bug 分支只用于本地修改bug 没必要推到远程
多人协作的时候 大家都会往master 和dev 分支上推送各自的修改

git remote //查看远程库信息  origin远程仓库的默认名称
git remote -v //查看远程库详细信息. 没有推送权的话 看不到push的地址
git push origin master //把该分支的所有本地推送到远程库
git push origin dev //推送dev 分支

git clone git@github.com:jingjing9061/test.git //从远程库克隆一个到本地库
git pull //抓取远程最新提交
git branch --set-upstream branch-name origin/branch-name //建立本地分支和远程分支的关联

推送失败
你的伙伴和你同时修改了文件，且他已往dev/master推送了他的提交，此时你推送文件会失败
此时需要先把最新的提交git pull抓下来，然后本地合并，解决冲突，再推送

git rebase //把本地未push的分叉提交历史整理成直线

创建标签：
git tag <name>就可以打一个新标签：
$ git tag v1.0
git tag -a v0.1 -m “version 0.1 released” 1094adb // -a 指定标签名 -m指定说明文字
//对gitlog 历史提交f52c633打上标签 
git tag v0.9 f52c633

git show <tagname> //查看标签信息
git tag//查看所有标签
git tag -d v0.1 //删除本地标签
git push origin v1.0 //把v0.1标签推送到远程
git push origin —tags//推送所有本地所有标签
删除远程标签 先从本地删除 
先 git tag -d v0.9
然后 git push origin :refs/tags/v0.9



vi readme.txt 进入命令文本编辑模式
退出编辑模式 
  按ESC键，然后：
    退出vi
   :q!  不保存文件，强制退出vi命令
    :w   保存文件，不退出vi命令
    :wq  保存文件，退出vi命令

遇到的错误解放方法：

错误1
我用git add file添加文件时出现这样错误：
fatal: Not a git repository (or any of the parent directories): .git
提示说没有.git这样一个目录，解决办法如下：
git init就可以了

错误2
git push -u origin master
To github.com:jingjing9061/learngit.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:jingjing9061/learngit.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
现这个错误的原因是是 因为你有远程库中的文件没有下载下来。所以你需要先运行
git pull origin master
然后你就看到了远程文件已经下载到你的工程里面并且自动合并了。
然后你需要在本地库中添加新文件并且提交。
然后你运行
git push -u origin master.

错误3 git无法pull仓库refusing to merge unrelated histories
两个不同的项目合并，git需要添加一句代码，在git pull

git pull origin master --allow-unrelated-histories



错误提示： 执行 git reset HEAD
Unstaged changes after reset 
解决的办法如下2中办法：
git add .
git reset --hard
2. 
git stash
git stash drop
出现这种现象的原因好像是因为在新分支上，repos没有感知不到这个阶段的改变，你要用 add 或 stash, 让其知晓，才能做想要的回滚。


错误：
fatal: remote origin already exists.
 git remote rm origin 




