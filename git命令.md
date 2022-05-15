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
git push -u origin "master"
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

## 18.查看分支列表

```shell
git branch
```

## 19.创建分支

```shell
git branch 分支名字
```

## 20.切换分支

```shell
git checkout 分支名字
```

## 21.快速创建并切换分支

```shell
git checkout -b 分支名字
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
git pull

git pull origin 分支名字
```

## 28.删除远程仓库

```shell
# 删除远程仓库中，制定名称的远程分支
git push 远程仓库名称 --delete 远程分支名称

# 示例
git push origin --delete login
```

## 29.强制删除

```shell
git branch -D 分支名字
```
