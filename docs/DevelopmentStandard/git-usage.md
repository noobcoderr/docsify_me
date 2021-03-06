## Git简单介绍

## Git常用场景

记录我理解的以及我使用的业务中场景的处理办法，如果有错误的或者有差异的还请指正

### 1、拉取远程项目的目标develop分支

`git clone` 项目下来通常只包含master分支，这个时候又不想使用`git fetch all`拉取所有分支下来，而只想拉取指定的分支。

可以使用

`git checkout -b develop origin/develop `

这样就切换到想要的分支了，并且已经拉取到了对应分支的代码


2、我在自己的feature分支进行开发，但是develop分支产生了改动，我需要更新下来，
	
	1、直接merge
	2、先git stash缓存，再merge，再pop出来，然后解决冲突
	3、git pull --rebase

3、我需要为我的功能新建一个分支，并且同步到远程
	
	git checkout -b new_branch
	本地创建的分支远程还未存在，所以需要将本地分支和远程分支关联起来，有2种方法
	1、提前建立连接,后续直接推
		git branch --set-upstream-to=origin/new_branch  new_branch
		git push
	2、commit后，推送到远程的时候再关联
		git push --set-upstream origin new_branch(远程分支名)


4、我被分配到了一个功能开发需求，需要
	
	创建一个新的特性分支，并在该特性分支上进行开发
		git checkout -b my_feat origin/my_feat
		git add 
		git commit 
	
	功能开发完之后，我需要将当前分支合并到develop公共分支，为了使得提交美观，使用rebase将所有提交合并为1个提交

5、master版本为10，但是我想回退到版本9
	
	git reset --hard 版本9的commit_id
	前提是要提交未push到远程,否则
	本地再稍微修改生成一次新的提交
	再git push --force
	但是如果目标是受保护的分支，那么--force不生效

6、特性分支开发完毕后，删除掉该分支
	
	先删除本地的分支 git branch -D feat/xxxx
	再删除远程的分支 git push origin --delete feat/xxxx
	参考：https://chinese.freecodecamp.org/news/how-to-delete-a-git-branch-both-locally-and-remotely/

7、生成commit log工具
	
	conventional-changelog，需要使用npm安装

8、git rebase之后，合并的提交其message不规范，需要重新修改message
	
	git commit --amend
	参考：https://blog.csdn.net/zxc024000/article/details/108939049

9、本地代码以及改的面目全非，拉取远程最新代码覆盖本地，且不合并冲突
	
	git fetch --all
	git reset --hard origin/feature/当前分支
10、其他
1、在仓库内初始化生成changelog的配置，选择风格、模板
git-chglog --init

2、master分支产生提交，打tag
git tag -a v1.0.1 -m "feat(channels): add a new channel"  

3、执行生成changelog
git-chglog -o CHANGELOG.md



2种开发功能，2种合并方式

1、做一个系统，有计划地进行版本迭代，如1.0、2.0、3.0等
master分支为生产分支
develop分支为开发测试分支
feature分支为特性分支

这种情况下，每个人会在需求讨论结束后，分配到一定的需求任务，有共同的上线时间，此时就需要根据需求创建对应的feature分支(基于develop分支)，在feat分支开发完后，要先拉取最新的develop分支代码，在feat分支解决完冲突之后，再切到develop分支merge feat，
当所有功能合并进develop分支，且在develop分支完成功能测试验收之后，再由develop向master分支发起pull-request，统一合并进入master


2、一个已有系统，没有明确的版本迭代进度，有需求就往里加的，

在这种情况下，需求是断断续续的，产生一个需求，指派给对应的人之后，此时也需要创建对应的feature分支，但是是基于master分支的，在feat分支开发完后，feat分支不能拉取最新的develop分支，而是要切换到develop分支，执行merge feat，解决冲突，完成合并。然后develop分支功能测试验收完毕后，对master分支执行rebase操作，由feat分支向master分支发起pull-request合并，统一合并进入master

不同点
1个基于master、1个基于develop产生feat分支
前者的feat要包含最新的develop分支代码，而后者则不能将develop分支内的代码拉取到feat内

## 参考
[Commit message 和 Change log 编写指南-阮一峰](https://www.nowcoder.com/ta/sql)

[git-flow 的工作流程](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow/)


git clone --mirror和git clone --bare之间的区别是什么
	https://www.imooc.com/wenda/detail/579025

gogs迁移gitlab
	https://www.cnblogs.com/zy0209/p/11265158.html

搞懂git rebase
	http://jartto.wang/2018/12/11/git-rebase/


gitflow with rebase
	https://gist.github.com/markreid/12e7c2203916b93d23c27a263f6091a0

gitflow
	https://githubflow.github.io/

git-hooks
	https://segmentfault.com/a/1190000040370691
	https://yanghongfei.github.io/2019/03/25/git-hooks/

github actions：cicd
	https://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html

git使用的一些姿势
	https://www.jianshu.com/p/393d8630dafa

gitlab自动部署测试服文档
	https://avohk9xvwg.feishu.cn/docs/doccnkgpL6faNMuZqSojaP7lkwe#VTfe5A

《hugo使用github action自动部署到github pages》
	https://tomial.github.io/posts/hugo%E4%BD%BF%E7%94%A8github-action%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E5%8D%9A%E5%AE%A2%E5%88%B0github-pages/
