Git 是目前最流行的分布式版本控制软件
## 一、版本管理
在开发的过程中用于管理对文件、目录或工程等内容的修改历史，方便查看历史记录，备份以便恢复以前的版本的软件工程技术
### 1. 本地版本控制
记录文件每次的更新, 可以对每个版本做一个快照, 或是记录补丁文件, 适合个人用, 如RCS
![[Pasted image 20230613194402.png]]
### 2. 集中版本控制
1. 所有的版本数据都保存在服务器上，协同开发者从服务器上同步更新或上传自己的修改
2. 用户的本地只有自己以前所同步的版本，如果不连网的话，用户就看不到历史版本，也无法切换版本
3. 所有数据都保存在单一的服务器上，如果这个服务器会损坏 (有很大的风险), 这样就会丢失所有的数据，需要定期备份
![[Pasted image 20230613194744.png]]

### 3. 分布式版本控制 
1. 所有版本信息仓库全部同步到本地的每个用户
2. 可以在本地查看所有版本历史，可以离线在本地提交，只需在连网时 push 到相应的服务器或其他用户那里。
3. 每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没有问题就可以恢复所有的数据
4. 更加安全, 不会因为服务器损坏或者网络问题，造成不能工作的情况
![[Pasted image 20230613194908.png]]

### 4. Git vs SVN
- SVN 
	- 是集中式版本控制系统，版本库是集中放在中央服务器的
		- 而工作的时候，用的都是自己的电脑，所以首先要从中央服务器得到最新的版本
		- 完成工作后，需要把自己的代码送到中央服务器。
	- 集中式版本控制系统是必须联网才能工作
- Git
	- Git 是分布式版本控制系统
		- 每个人的电脑就是一个完整的版本库，工作的时候不需要联网了，因为版本都在自己电脑上。
	- 协同的方法说明：
		- 比如自己在电脑上改了文件 A，其他人也在电脑上改了文件 A
			- 这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了
			- Git 可以直接看到更新了哪些代码和文件

## 二、Git 下载&安装
[详细参考(廖雪峰Git教程)](https://www.liaoxuefeng.com/wiki/896043488029600/896067074338496)
[[Git 常用命令]]
## 三、Git 工作原理
![[Pasted image 20230613202304.png]]
- 四个工作区域:
	- Git 本地有三个工作区域：
		- 工作目录（Working Directory）
		- 暂存区 (Stage/Index)
		- 资源库 (Repository 或 Git Directory)
	- 远程的 git 仓库 (Remote Directory)

![[Pasted image 20230613202905.png]]
- `Directory`：使用 Git 管理的一个目录，也就是一个仓库，包含我们的工作空间和 Git 的管理空间。 
- `WorkSpace`：需要通过 Git 进行版本控制的目录和文件，这些目录和文件组成了工作空间。 
- `.git`：存放 Git 管理信息的目录，初始化仓库的时候自动创建。 
- `Index/Stage`：暂存区，或者叫待提交更新区，在提交进入 repo 之前，我们可以把所有的更新放在暂存区。 
- `Local Repo`：本地仓库，一个存放在本地的版本库；
- `HEAD` 只是当前的开发分支 (branch)。 
- `Stash`：隐藏，是一个工作状态保存栈，用于保存/恢复 WorkSpace 中的临时状态。

## 四、时光机穿梭
要随时掌握工作区的状态，使用 `git status` 命令。

如果 `git status` 告诉你有文件被修改过，用 `git diff` 可以查看修改内容。

### 1. 版本回退
命令显示从最近到最远的提交日志
```shell
git log --pretty=oneline

13158a8cb06a47c3bd1d7f537d584eac5d248122 (HEAD -> master) No.4 add
3ccb15a945ca69db88c75395ba5281a5f3e8501e No.3 add
0ab43b5a3fe6e60c0a94685a8cf4fc0ed73146b0 No.2 add
655e99020188e41dce2eb4f48b582a2cf7ff9481 No.1 提交
```
启动时光穿梭机，准备把 `readme.txt` 回退到上一个版本 
`git reset --hard HEAD^`
```shell
HEAD is now at 3ccb15a No.3 add
```
查看日志
```shell
git log --pretty=oneline

3ccb15a945ca69db88c75395ba5281a5f3e8501e (HEAD -> master) No.3 add
0ab43b5a3fe6e60c0a94685a8cf4fc0ed73146b0 No.2 add
655e99020188e41dce2eb4f48b582a2cf7ff9481 No.1 提交
```
此时已经看不到 `No.4 add` 的版本了, 可以根据 id 从过去回到现实
```shell
git reset --hard 13158a8

HEAD is now at 13158a8 No.4 add
```
![[Pasted image 20230616100411.png]]

Git 提供了一个命令 `git reflog` 用来记录你的每一次命令

>[!note] 小结
> - HEAD 指向的版本就是当前版本，因此，Git 允许我们在版本的历史之间穿梭，使用命令 `git reset --hard commit_id`
> - 穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本
> - 要重返未来，用 `git reflog` 查看命令历史，以便确定要回到未来的哪个版本

### 2. 撤销修改
1. 没有 `git add` 时，用 `git checkout -- file`
2. 已经 `git add` 时，先 `git reset HEAD <file>` 回退到1，再按1操作
3. 已经 `git commit` 时，用 `git reset` 回退版本

### 3. 删除文件
先 `git add test.txt` 添加文件, 再删除工作区文件 `rm test.txt`
但是版本库并没有删除需要用命令 `git rm` 删掉，并且 `git commit`
另一种情况就是工作区删错了, 使用 `git checkout -- test.txt` 恢复文件
>[!Warning]  注意：从来没有被添加到版本库就被删除的文件，是无法恢复的！

## 五、远程仓库
>[!note]- 配置 SSH 连接
> 创建密钥 `ssh-keygen -t rsa -C "youremail@example.com"`
> 打开目录所在 `cd ~/.ssh`,复制公钥 `cat id_rsa.pub`
> `Gitee`、`GitHub` 设置账号配置 SSH

>[!note]- 添加远程库
> 1. 登陆 GitHub, 创建仓库
> 2. 推送本地仓库
> 	```shell
> 	git remote add origin git@github.com:yuxiang01/study.git
> 	git branch -M main
> 	git push -u origin main
> 	```

>[!note]- 删除远程库
> 1. 使用前，建议先用 `git remote -v` 查看远程库信息
> 2. 删除远程库 `git remote rm origin`
> 	此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库

>[!note] 从远程库克隆: git clone 克隆一个本地库 

## 六、分支管理
### 1. 创建与合并分支
1. 首先，我们创建 dev 分支，然后切换到 dev 分支：`git checkout -b dev`
	- git checkout 命令加上-b 参数表示创建并切换，相当于以下两条命令：
		- `git branch dev` + `git checkout dev`
2. 然后，用 `git branch` 命令查看当前分支
	- `git branch` 命令会列出所有分支，当前分支前面会标一个 `*` 号 
3. 对 `readme.txt` 做个修改, 然后提交, 再切回 `git checkout master` 分支
	![[Pasted image 20230616120820.png]]
4. 把 dev 分支的工作成果合并到 master 分支上 `git merge dev` 
5. 合并完成后，就可以放心地删除 `dev` 分支了：`git branch -d dev`
6. 实际上，切换分支这个动作，用 `switch` 更科学, 创建并切换到新的 `dev` 分支 `git switch -c dev`

>[!note] 小结
> 查看分支：`git branch`
> 创建分支：`git branch <name>`
> 切换分支：`git checkout <name>` 或者 `git switch <name>`
> 创建+切换分支：`git checkout -b <name>` 或者 `git switch -c <name>`
> 合并某分支到当前分支：`git merge <name>`
> 删除分支：`git branch -d <name>`

### 2. 解决冲突
新建分支 `git switch -c v1.0`
修改 `readme.txt` 内容 `Creating a new branch is quick AND simple.`
在 `分支v1.0` 上提交 `git add readme.txt`、`git commit -m "提交 v1.0 code"`

---
切换到 master 分支 `git switch master`
在 `master` 分支上把 `readme.txt` 文件的最后一行改为 `Creating a new branch is quick & simple`
在 `分支 master` 上提交修改 `git add readme.txt`、`git commit -m "修改 master 内容"`

![[Pasted image 20230616155646.png]]

---
`git merge v1.0` 出现冲突
然后手动处理冲突, 再提交 `git add .`、`git commit -m "修改冲突"`
用带参数的 git log 也可以看到分支的合并情况：`git log --graph --pretty=oneline --abbrev-commit`
### 3. 分支管理策略
通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用 `Fast forward` 模式，Git 就会在 merge 时生成一个新的 commit，这样，从分支历史上就可以看出分支信息。

---
- 首先，仍然创建并切换 `dev` 分支： `git switch -c dev`
- 修改 `readme.txt` 文件，并提交一个新的 commit：`git add readme.txt` 、`git commit -m "add merge"`
- 现在，我们切换回 `master`：`git switch master`
- 准备合并 `dev` 分支，请注意 `--no-ff` 参数，表示禁用 `Fast forward`：
	- 因为本次合并要创建一个新的 commit，所以加上 `-m` 参数，把 commit 描述写进去。
		- `git merge --no-ff -m "merge with no-ff" dev`
- 合并后，我们用 `git log --graph --pretty=oneline --abbrev-commit` 看看分支历史 
- 可以看到，不使用 `Fast forward` 模式，merge 后就像这样：
	![[Pasted image 20230616175404.png]]

### 4. Bug 分支
修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除； 
当手头工作没有完成时，先把工作现场 `git stash` 一下，然后去修复 bug，修复后，再 `git stash pop`，回到工作现场； 
在 master 分支上修复的 bug，想要合并到当前 dev 分支，可以用 `git cherry-pick <commit>` 命令，把 bug 提交的修改“复制”到当前分支，避免重复劳动。 

### 5. Feature 分支
开发一个新 feature，最好新建一个分支
```shell
git switch -c feature-vulcan
```
如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。
### 6. 多人协作
当你从远程仓库克隆时，实际上 Git 自动把本地的 `master` 分支和远程的 `master` 分支对应起来了，并且，远程仓库的默认名称是 `origin`
要查看远程库的信息，用 `git remote -v`,如果没有推送权限，就看不到 push 的地址
#### 1. 推送分支
Git 就会把该分支推送到远程库对应的远程分支上：
```shell
git push origin master
## 如果要推送其他分支，比如dev
git push origin dev
```
但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

#### 2. 抓取分支
- 多人协作的工作模式通常是这样：
	1. 首先，可以试图用 `git push origin <branch-name>` 推送自己的修改；
	2. 如果推送失败，则因为远程分支比你的本地更新，需要先用 `git pull` 试图合并；
	3. 如果合并有冲突，则解决冲突，并在本地提交；
	4. 没有冲突或者解决掉冲突后，再用 `git push origin <branch-name>` 推送就能成功

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`

---
- 查看远程库信息，使用 `git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

---
本地 master 分支和远程 master 分支同步，本地 dev 和远程 dev 同步，名字只是一个代号，切忌本地 master 同步远程 dev 这种，自己把自己绕进去。
你在本地dev开发，和远程dev同步是你的责任，不然其他人没法拿到你的commit
是否要把本地dev合并到本地master，看你的策略，如果每个commit你都立刻合并到master然后和远程同步，那要两个分支就没意义了。
正常开发是在某个大功能稳定是，由某个人把 dev 合并到 master，同步远程 master，最好打上 v1.1，v1.2的标签，便于跟踪

### 7. Rebase
- rebase 操作可以把本地未 push 的分叉提交历史整理成直线；`git rebase`
- rebase 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 七、标签管理
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起
### 1. 创建标签
- 命令 `git tag <tagname>` 用于新建一个标签，默认为 `HEAD`，也可以指定一个 commit id；
- 命令 `git tag -a <tagname> -m "blablabla..."` 可以指定标签信息； 
- 命令 `git tag` 可以查看所有标签

### 2. 操作标签
- 命令 `git push origin <tagname>` 可以推送一个本地标签；
- 命令 `git push origin --tags` 可以推送全部未推送过的本地标签； 
- 命令 `git tag -d <tagname>` 可以删除一个本地标签； 
- 命令 `git push origin :refs/tags/<tagname>` 可以删除一个远程标签。 

![[Pasted image 20230831112815.png]]