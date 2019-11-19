本地仓库由 git 维护的三棵“树”组成。
第一个是 工作目录，它持有实际文件；
第二个是 缓存区（Index），它像个缓存区域，临时保存你的改动；
最后是 HEAD，指向你最近一次提交后的结果。

1.基础配置
git config --global user.name "liqiang"
git config --global user.email "xxx@qq.com"
2.在指定的文件夹(存储文件夹)下执行
git init
3.链接项目地址
git remote add origin <你的项目地址> //注:项目地址形式为:https://gitee.com/xxx/xxx.git或者 git@gitee.com:xxx/xxx.git
eg：git remote add origin git@git.rd.com:xiaodai_xiangmu/dsxd.git
4.生成ssh秘钥
查看秘钥是否存在：type %userprofile%\.ssh\id_rsa.pub
生成秘钥：ssh-keygen -t rsa -C "liq@erongdu.com"
密钥存放地址：C:\Users\Administrator\.ssh   id_rsa为秘钥 id_rsa.pub为公钥
把公钥调价到git管理器上去
5.克隆项目（在想要存放的目录下打开 git bash）
git clone <项目地址>
eg：git clone git@git.rd.com:xiaodai_xiangmu/dsxd.git

1.把计划改动（把它们添加到缓存区），使用如下命令：
    git add <filename>
    git add *
	
2.使用如下命令以实际提交改动，改动已经提交到 HEAD，但是还没到你的远端仓库。
    git commit -m "代码提交信息"

3.执行如下命令以将这些改动提交到远端仓库：
git push origin master


查看文件状态
git status

查看文件的修改内容
git diff readme.txt

显示从最近到最远的提交日志
git log [--pretty=oneline]

回退版本 上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写成HEAD~100。
git reset --hard HEAD^

回复到指定版本
git reset --hard 版本号（前几位即可）

用来记录你的每一次命令
git reflog

撤销操作
想直接丢弃工作区的修改：git checkout -- file
添加到了暂存区时想丢弃修改：git reset HEAD <file>
提交过后撤销：git reset --hard HEAD^
提交到远程库不可撤销

删除文件
git rm test.txt    再提交    git commit -m "说明文字"

克隆一个本地库
git clone git@git.rd.com:xiaodai_xiangmu/dsxd.git



分支操作：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>


创建dev分支，然后切换到dev分支
git checkout -b dev
-b参数表示创建并切换，相当于以下两条命令
git branch dev
git checkout dev

查看当前分支 git branch命令会列出所有分支，当前分支前面会标一个*号。
git branch

切换回master分支
git checkout master

合并指定分支到当前分支 当前为master，即dev合并到master
git merge dev

删除dev分支 （没有合并但想强行删除，需要使用大写的-D参数）
git branch -d dev

显示分支合并图的命令
git log --graph

准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward
git merge --no-ff -m "merge with no-ff" dev
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去

stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
git stash

git status 查看工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了


查看远程库的信息 -v参数：显示更详细的信息
git remote [-v]

推送分支
git push origin master

创建本地dev分支，远程origin的dev分支到本地
$ git checkout -b dev origin/dev

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，
解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，
然后，在本地合并，解决冲突，再推送
git pull


多人协作的工作模式通常是这样：

1.首先，可以试图用git push origin <branch-name>推送自己的修改；

2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3.如果合并有冲突，则解决冲突，并在本地提交；

4.没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

5.如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。


查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


创建标签
1.切换到需要打标签的分支上 
git branch  //查看分支列表
git checkout master   //选择分支

2.敲命令git tag <name>就可以打一个新标签
git tag v1.0

3.可以用命令git tag查看所有标签

忘记给以前的内容打标签该如何操作？
1.找到历史提交的commit id
git log --pretty=oneline --abbrev-commit

2.对某个commit_id打上标签
git tag v0.9 f52c633

3.再用命令git tag查看标签

附加说明：
  还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
  git tag -a v0.1 -m "version 0.1 released" 1094adb

  标签不是按时间顺序列出，而是按字母排序的，查看标签信息用以下命令，可以看到说明文字
  git show <tagname>   eg：git show v0.9
  
注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

删除标签
git tag -d v0.1

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。
如果要推送某个标签到远程，使用命令git push origin <tagname>：
git push origin v1.0

或者，一次性推送全部尚未推送到远程的本地标签：
git push origin --tags

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
$ git tag -d v0.9

然后，从远程删除。删除命令也是push，但是格式如下：
$ git push origin :refs/tags/v0.9





