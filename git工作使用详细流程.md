# git工作中使用详细流程

## 一、了解git

1. git分为4个区域：

	```shell
	工作区==>暂存区==>本地仓库==>远程仓库
	```

	

	在日常工作中要新建项目，使用git管理流程如下

	- 创建文件夹，打开命令窗口，定位到当前文件夹，使用`git init`命令初始化，这是该文件夹已经被git接管

	- 如果是第一次使用git需要设置git用户名，邮箱（不用真实邮箱符合邮箱格式即可）

		```shell
		git config --global user.name "username" #设置用户名，把引号的名字替换成自己的
		git config --global user.email "email" #设置邮箱，把引号的邮箱换成自己的
		git config user.name #查看用户名
		git config user.email #查看邮箱
		```

	- 此时向文件夹（***`后面统称项目`***）内写入工作代码，运行`git add .`将新增的代码文件从工作区添加到暂存区，此时使用 `git status`将会看到文件状态的改变

	- 运行`git add .` 后 运行 `git commit -m '提交信息'`，此时已经将代码保存到本地仓库

		```shell
		git commit -m '提交信息-请设置有意义的信息防止代码回退时找不到'
		```

		

2. 此时文件已经从**工作区**进入到**暂存区**保存到**本地仓库**流程走通了一遍，在工作中要理解各个区域的关系灵活使用

3. 将代码上传到本地仓库

	- 要上传到远程仓库，首先要有一个远程仓库，可以使用github 或 gitee等平台创建仓库获取仓库地址

	- 有了远程仓库地址，要在本地设置

		```shell
		git remote add origin 你的仓库链接
		```

	- 将本地仓库推送到远程仓库

		```shell
		git push -u origin "master" #第一次使用要带 -u 参数表示将本地仓库和远程从仓库进行关联
		git push -u 远程仓库别名 本地分支名:远程分支名 #命令详细解释，由于第一次推送远程仓库没有任何分支所以上一条命令仓库别名后面是 "master" 表示将本地master分支推送到远程仓库，远程仓库会创建出master分支
		#以上命令在第一次运行过后下次提交直接使用 git push 命令
		```

		

## 二、工作中使用

#### 1. 工作流程

- 要加入一个项目，你会得到一个git远程仓库链接使用git clone命令将项目克隆下来

	```shell
	git clone 仓库链接
	```

- 在vs code中打开项目查看项目中使用的是什么包管理器，一般会在Readme.md文件中进行说明，使用错误的包管理器可能会引发错误

	```javascript
	//如果Readme.md文件中没有进行说明，则查看项目根目录下是否有一下文件其中一个
	package-lock.json  //使用npm包管理器
	yarn-lock.json //使用yarn包管理器
	//包管理器不止这一种，还有cnpm pnpm等 yarn 和 yarn2管理方式也不同可以视为不同的包管理器
	```

	

- 根据项目使用的包管理器下载项目依赖，以npm为例

	```shell
	npm install
	npm i #简写
	```

- 依赖下载完毕，使用`git switch -c dev`（dev是分支名自己起）命令创建开发分支，此时本地的两个分支和远程仓库代码是同步的

- 在dev分支开发完自己的任务后需要在走一遍 `git add . git commit -m ''`切换回master分支

- 在master分支中先将远程仓库代码拉到本地(**要同步到远程仓库前先告知其他开发人员，避免冲突**)

	```shell
	git fetch origin master # 将远程仓库代码拉到本地临时分支
	git diff FETCH_HEAD #进行比较，如果没有改变直接合并自己的dev分支，然后git push！！！
	git merge FETCH_HEAD # 如果有改动合并临时分支
	git merge dev # 合并自己开发完成的代码到主分支
	git push #直接推送，推送完毕立马回到开发分支！！！
	git switch dev #回到开发分支
	git merge master #因为master分支之最新的，所以要同步master代码到dev分支这一步很重要！！！
	```

- 继续开发

#### 2. 代码回滚

- 在实际开发中可能会出现，自己合并失误，或自己写的代码上线后有问题需要代码回滚的问题

##### 2.1  在本地仓库合并远程仓库出错，还**`未psh`**到远程仓库（这里的本地仓库无特殊说明指的是master分支）

``` shell
git reflog # 找到要回退的版本，一般都是倒数第二条记录，复制commitID
git reset --hard commitID # 回退到指定版本并删除提交记录 commitID不是命令，结合上文看得懂吧
git merge dev #一般都是拿远程最新分支合并自己开发的功能代码冲突的时候出错，所以这里合并dev分支，灵活运用，合并谁出错的，这里就再次合并谁
git add . # 处理完冲突要在执行一遍流程
git commit -m ''
git push #最后push
git switch dev #不解释了
git merge master #不解释了
```

##### 2.2`已经push`到远程仓库

自己是最后提交的，要恢复提交之前，没有合并之前的代码以下示例

```javascript
A提交的==>你在dev里提交的记录==>你的错误提交
```



```shell
git reflog #找到要回退的版本,复制commitID，应该是A提交的
git revert commitID #撤销了自己的提交，别人的代码还在
git push -r #强制推送远程分支将远程分支回滚，此时你和别人的代码仓库和远程代码仓库同步
git switch -c temp #创建一个分支，
git reflog #不解释
git reset --hard commitID #回滚到dev分支正确的记录， 应该是你在dev里的提交记录
git switch master #切回主分支
git merge temp #合并temp分支，temp分支是自己合并错误之前写的代码
git push
git branch -d temp #删除分支
git switch dev #切回dev分支
git merge master
```

自己的代码出了问题，要回退到之前的版本，中间有别人提交的代码以下示例

```javascript
你之前没bug的代码==>A提交的==>你出问题的代码
```

你要做的

```shell
git reflog #找到要回退的版本,复制commitID，应该是你之前没bug的代码
git revert commitID #撤销了自己的提交，别人的代码也没了
git push -r #强制推送远程分支将远程分支回滚，此时要告知别人代码回滚了，别人执行 git pull 本地代码也进行了回滚
git switch dev #切回开发分支修改bug,之后的流程回归正常
```

其他人要做的，也有可能你是其他人

```shell
git pull #跟远程仓库保持一致
git reflog #找到自己最后一次提交代码的commitID
git reset --hard # 版本恢复
git switch -c temp #创建分支保存,这是temp保存的是自己最后一次提交的代码 A提交的
git switch master #切回主分支
git reset --hard #回到自己代码最前端
git reset --hard origin/master # 和远程保持同步
git merge temp #将代码恢复
git branch -d temp # 删除分支
git push
```

#### 3. 回滚命令

revert命令

| 回滚命令            | 说明                                                        |
| ------------------- | ----------------------------------------------------------- |
| git revert HEAD     | 撤销最近一次提交，并保留之前的commit记录                    |
| git revert HEAD~1   | 撤销上上次的提交，注意：数字从0开始，并保留之前的commit记录 |
| git revert commitID | 撤销commitID指定提交，并保留之前的commit记录                |

reset命令,不保留commit记录，是真正意义上的回退危险操作

| 回滚命令                  | 说明                                       |
| ------------------------- | ------------------------------------------ |
| git reset HEAD^           | 回退所有内容到上一个版本                   |
| git reset HEAD^ hello.php | 回退 hello.php 文件的版本到上一个版本      |
| git  reset  commitID      | 回退到指定版本，并将未提交内容回退到工作区 |
| git reset --soft          | 撤销提交到暂存区                           |
| git reset --mixed         | 撤销提交到工作区                           |

## 三、以下情况要注意保存工作区

1. 当前功能还没开发完，需要修复之前的bug,这时要 `git stash -m '信息'` 保存工作区
2. mastert分支要与远程同步，dev分支的代码还未开发完，要 `git stash save ‘信息’` 保存工作区
3. 要在不同分支开发不同功能，切换分支前要注意保存当前分支的工作区并记住stash记录

**特别注意工作区中的内容，没有保存，切换分支会被带到另一个分支中**

如果你在dev分支开发的功能还未提交，这时切换回了master分支，从远程仓库中pull到了master分支，这时**工作区内容全部丢失，无法找回**

## git提交信息规范

- **feat:** 新功能（feature）
- **fix:** 修复bug
- **docs:** 文档相关的变更
- **style:** 代码风格、格式的调整，不影响代码逻辑
- **refactor:** 代码重构
- **test:** 测试相关的变更
- **chore:** 无关紧要的变更，例如构建工具、依赖库的升级
- **ci:** 持续集成配置的变更
- **merge**: 合并

```git
git commit -m 'feat(模块): 新增后台管理模块'
git commit -m 'fix(修复统计): 修复统计错乱情况'
git commit -m 'refactor(任务管理模块重构): 任务管理模块重构'
```

### 规范正则

```text
^(feat|fix|docs|style|refactor|perf|build|revert|wip|workflow|types|release|test|chore|merge|ci)(\(.{0,20}\))?: .{1,108}
```

