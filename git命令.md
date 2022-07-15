# Git命令

## 1.项目的初始化

```shell
git init
```

## 2.查看文件状态

```shell
git status
git status -s #精简显示状态
```

## 3.添加到暂存区

```shell
git add 文件名字
```

## 4.提交到本地仓库

```shell
git commit -m '描述信息'
```

## 5.撤销对文件的修改

```shell
git checkout -- 文件名
#新方法
git restore 文件名
```

## 6.一次性添加多个文件到暂存区

```shell
git add .
```

## 7.取消暂存的文件

```shell
git reset HEAD 文件名字
git reset HEAD . # 取消所有的暂存文件
```

## 8.跳过暂存区

```shell
git commit -a -m '描述信息' # 只有修改的文件才可以跳过
```

## 9.移除文件

```shell
# 从 Git仓库和工作区中同时移除 index.js 文件
git rm -f index.js
# 只从 Git 仓库中移除 index.css，但保留工作区中的 index.css 文件
git rm --cached index.css
```

## 10.查看提交历史

```shell
# 按时间先后顺序列出所有的提交历史，最近的提交在最上面
git log

# 只展示最新的两条提交历史，数字可以按需进行填写
git log -2

# 在一行上展示最近两条提交历史的信息
git log -2 --pretty=oneline

# 在一行上展示最近两条提交历史信息，并自定义输出的格式
# &h 提交的简写哈希值  %an 作者名字  %ar 作者修订日志  %s 提交说明
git log -2 --pretty=format:"%h | %an | %ar | %s"
```

## 11.回退指定版本

```shell
# 在一行上展示所有的提交历史
git log --pretty=oneline

# 使用 git reset --hard 命令，根据指定的提交 ID 回退到指定版本
git reset --hard <CommitID>

# 在旧版本中使用 git reflog --pretty=oneline 命令，查看命令操作的历史
git reflog

# 再次根据最新的提交 ID，跳转到最新的版本
git reset --hard <CommitID>
```

## 12.设置云仓库的链接

```shell
git remote add origin 你的仓库链接
```

## 13.将本地代码推送到云仓库

```shell
git push -u origin "master"or
```

## 14.移除链接

```shell
git remote remove origin
```

## 15.生成sshkey

```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## 16.测试sshkey是否成功

```shell
ssh -T git@gitee.com
```

## 17.克隆代码到本地

```shell
git clone 你的仓库链接
```

## 18.查看分支列表,查看分支从谁分出来的

```shell
git branch
#
git reflog show --date=iso 分支名
# 查看merge和checkout记录
git reflog show --date=local | grep 分支
名
```

## 19.创建分支

```shell
git branch 分支名字
```

## 20.切换分支

```shell
git checkout 分支名字
#创建切换分支
git checkout -b 分支名字
#新的方法,建议使用这个
git switch 分支名字
```

## 21.快速创建并切换分支

```shell
git checkout -b 分支名字
#新的方法,建议使用这个
git switch -c分支名字
#设置主分支
git branch -M 分支名
```

## 22.合并分支

```shell
git merge 分支名字
```

## 23.删除分支

```shell
git branch -d  分支的名字  #需要是合并过的才能删除
```

## 24.将本地分支推送到远程仓库

```shell
#把本地分支推送到远程分支,并进行关联
git push --set-upstream origin master:master
# -u 表示把本地分支和远程分支进行关联，只在第一次推送的时候需要带 -u 参数
git push -u 远程仓库的别名 本地分支名称:远程分支名称

# 实际案例
git push -u origin login:abc

# 如果希望远程分支的名称和本地分支名称保持一致，可以对命令进行简化
git push -u origin login

#如果远程仓库和本地仓库文件不一致可以合并
git push -rebase origin master
```

## 25.查看远程分支列表

```shell
git remote show 远程仓库名称   #默认 origin
#查看远程分支列表简略信息
git branch -r
```

## 26.跟踪分支 (将远程分支下载到本地)

```shell
# 示例
git checkout login

# 从远程仓库中，把对应的远程分支下载到本地仓库，并把下载的本地分支进行重命名
git checkout -b 本地分支名称 远程仓库名称/远程分支名称

# 示例
git checkout -b login origin/loginer
```

## 27.拉取远程代码到本地

```shell
git pull origin 远程分支名:本地分支名

git pull origin 远程分支名字
```

## 28.删除远程仓库

```shell
# 删除远程仓库中，制定名称的远程分支
git push 远程仓库名称 --delete 远程分支名称
# 示例
git push origin --delete login
#把指定本地分支的 commit 推到指定的远程主机远程分支上
git push <远程主机名> <本地分支名>:<远程分支名>
#把本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。
git push <远程主机名> <远程分支名> 
#删除指定的远程分支，等同于推送一个空的本地分支到远程分支
git push <远程主机名>:<远程分支名>
#指定默认主机，下次直接 git push 即可
git push -u <远程主机名> <本地分支名>
```

## 29.强制删除

```shell
git branch -D 分支名字
```

## 30.合并远程分支

1.创建本地分支

```shell
#查看当前远程的版本
$ git remote -v 
#获取最新代码到本地临时分支(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
$ git fetch origin master:master1  [示例1：在本地建立master1分支，并下载远端的origin/master分支到master1分支中]
$ git fetch origin dev:dev1[示例1：在本地建立dev1分支，并下载远端的origin/dev分支到dev1分支中]
#查看版本差异
$ git diff master1 [示例1：查看本地master1分支与当前分支的版本差异]
$ git diff dev1    [示例2：查看本地dev1分支与当前分支的版本差异]
#合并最新分支到本地分支
$ git merge master1    [示例1：合并本地分支master1到当前分支]
$ git merge dev1   [示例2：合并本地分支dev1到当前分支]
#删除本地临时分支
$ git branch -D master1    [示例1：删除本地分支master1]
$ git branch -D dev1 [示例1：删除本地分支dev1]
```

2.不用创建本地分支

```shell
#查询当前远程的版本
$ git remote -v
#获取最新代码到本地(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
$ git fetch origin master  [示例1：获取远端的origin/master分支]
$ git fetch origin dev [示例2：获取远端的origin/dev分支]
#查看版本差异
$ git log -p master..origin/master [示例1：查看本地master与远端origin/master的版本差异]
$ git log -p dev..origin/dev   [示例2：查看本地dev与远端origin/dev的版本差异]
$ git log -p FETCH_HEAD [取回更新后会返回一个FETCH_HEAD,指的是某个分支的远程最新状态,查看最新状态]
#合并最新代码到本地分支
$ git merge origin/master  [示例1：合并远端分支origin/master到当前分支]
$ git merge origin/dev [示例2：合并远端分支origin/dev到当前分支]
$ git merge FETCH_HEAD [将拉取下来最新内容合并到当前分支]
```

## 31.暂时保存工作区文件,防止被带到其他分支

```shell
#保存工作区文件开始下一个分支的工作
git stash save "修改的信息"
#查看保存的文件列表
git stash list
#回到保存的文件版本
git stash apply stash@{0}
#删除指定id 的stash
git stash drop [stash_id]
#：删除所有缓存的 stash。
git stash clear 
```

## 32.git代理设置

```javascript
//设置全局代理
//http
git config --global https.proxy http://127.0.0.1:1080
//https
git config --global https.proxy https://127.0.0.1:1080
//使用socks5代理的 例如ss，ssr 1080是windows下ss的默认代理端口,mac下不同，或者有自定义的，根据自己的改
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080

//只对github.com使用代理，其他仓库不走代理
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
git config --global https.https://github.com.proxy socks5://127.0.0.1:1080
//取消github代理
git config --global --unset http.https://github.com.proxy
git config --global --unset https.https://github.com.proxy

//取消全局代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```

SSH协议

```javascript
//对于使用git@协议的，可以配置socks5代理
//在~/.ssh/config 文件后面添加几行，没有可以新建一个
//socks5
Host github.com
User git
ProxyCommand connect -S 127.0.0.1:1080 %h %p

//http || https
Host github.com
User git
ProxyCommand connect -H 127.0.0.1:1080 %h %p
```

## 33.查看分支图

```git
git log --graph --all
```

## 34.查看分支包含的文件

```git
//切换分支并且查看分支文件
git checkout 分支名 && ls
//查看当前分支的内容
ls
```
