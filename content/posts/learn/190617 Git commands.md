---
title: "Git常用命令汇总"
date: 2019-06-17
lastmod: 2019-06-17
author: ["沧海"]
tags: ["Git"]
comments: true
showToc: true
TocOpen: true
hidemeta: false
showbreadcrumbs: true
---
>初始配置

Git 装好之后要先配置用户名和邮箱，提交记录里显示的就是这两项。邮箱用的是 GitHub 提供的 noreply 邮箱，这样提交记录里不会暴露真实邮箱：

```bash
git config --global user.name "canghai0823"
```

```bash
git config --global user.email "canghai0823@users.noreply.github.com"
```

国内访问 GitHub 经常连不上，给 Git 单独配置代理：

```bash
git config --global http.proxy socks5h://127.0.0.1:10808
```

```bash
git config --global https.proxy socks5h://127.0.0.1:10808
```

不需要代理的时候取消掉：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

查看当前所有配置：

```bash
git config --list
```

`--global`是全局配置，对所有仓库生效。如果某个仓库要用不同的用户名邮箱，在仓库目录里去掉`--global`再执行一遍就行，仓库配置的优先级高于全局。

>创建仓库

在当前目录初始化一个新仓库：

```bash
git init
```

克隆远程仓库到本地：

```bash
git clone https://github.com/user/repo.git
```

克隆时可以指定目录名，也可以用`--depth 1`只拉最近一次提交，大仓库能省不少时间和流量：

```bash
git clone --depth 1 https://github.com/user/repo.git myrepo
```

>日常提交

查看当前状态，哪些文件改了、哪些没跟踪，都在这里看：

```bash
git status
```

把文件加入暂存区：

```bash
git add file.txt
```

加所有改动：

```bash
git add .
```

提交：

```bash
git commit -m "提交说明"
```

如果文件之前已经被跟踪过，可以用`-a`跳过`add`直接提交：

```bash
git commit -am "提交说明"
```

刚提交完发现说明写错了，或者漏了个文件，用`--amend`修改上一次提交：

```bash
git commit --amend -m "新的提交说明"
```

需要注意`--amend`会改掉提交记录，已经 push 过的提交不要再 amend。

>查看改动和历史

查看工作区和暂存区的差异：

```bash
git diff
```

查看暂存区和上次提交的差异：

```bash
git diff --cached
```

查看提交历史：

```bash
git log
```

历史太长的话，一行显示一条更清楚：

```bash
git log --oneline
```

加上`--graph`能看到分支合并的图：

```bash
git log --oneline --graph --all
```

查看某次提交具体改了什么：

```bash
git show 提交ID
```

>撤销操作

工作区改乱了，想恢复成上次提交的样子：

```bash
git checkout -- file.txt
```

已经`add`了想撤回暂存：

```bash
git reset HEAD file.txt
```

撤销整个提交有两种方式。`reset`直接把分支退回到某次提交，之后的提交记录就没了：

```bash
git reset --hard 提交ID
```

`--hard`会把工作区的改动一起丢掉，用之前想清楚。如果想保留改动只撤销提交记录，用`--soft`：

```bash
git reset --soft HEAD^
```

`revert`是生成一次新提交来抵消之前的提交，历史记录还在，已经 push 过的提交要撤销用这个更安全：

```bash
git revert 提交ID
```

如果`reset --hard`退过头了，可以用`reflog`找回来，它记录了 HEAD 的所有移动：

```bash
git reflog
```

>分支操作

查看分支，当前分支前面有个`*`：

```bash
git branch
```

创建分支：

```bash
git branch dev
```

切换分支：

```bash
git checkout dev
```

创建并切换，一步到位：

```bash
git checkout -b dev
```

把 dev 分支合并到当前分支：

```bash
git merge dev
```

合并出现冲突时，Git 会在冲突文件里用`<<<<<<<`和`>>>>>>>`标出两边的内容，手动改完之后`add`再`commit`就完成合并了。

删除已合并的分支：

```bash
git branch -d dev
```

没合并的分支用`-d`删不掉，确定不要了用`-D`强制删除。

>远程仓库

查看远程仓库地址：

```bash
git remote -v
```

添加远程仓库，`origin`是习惯用的名字：

```bash
git remote add origin https://github.com/user/repo.git
```

修改远程地址：

```bash
git remote set-url origin https://github.com/user/repo.git
```

第一次推送分支，加`-u`把本地分支和远程分支关联起来，以后直接`git push`就行：

```bash
git push -u origin master
```

之后的日常推送和拉取：

```bash
git push
git pull
```

`pull`相当于`fetch`加`merge`。如果只想把远程的更新拿下来先看看，不着急合并：

```bash
git fetch
```

>暂存现场

改到一半需要切分支处理别的事，改动还不想提交，可以先存起来：

```bash
git stash
```

查看存了哪些：

```bash
git stash list
```

回来之后恢复现场：

```bash
git stash pop
```

`pop`会恢复并删掉这条记录，如果想恢复但保留记录用`git stash apply`。

>标签

发版本的时候打个标签，方便以后找到对应的提交：

```bash
git tag v1.0
```

给之前的某次提交补标签：

```bash
git tag v0.9 提交ID
```

查看所有标签：

```bash
git tag
```

标签默认不会被 push 上去，要单独推：

```bash
git push origin v1.0
```

一次推所有标签：

```bash
git push origin --tags
```

删除本地标签：

```bash
git tag -d v1.0
```

>忽略文件

仓库根目录建一个`.gitignore`文件，写上不想跟踪的文件和目录，一行一条：

```
*.log
node_modules/
.DS_Store
```

需要注意`.gitignore`只对没跟踪过的文件生效，已经提交过的文件要先从暂存区移除：

```bash
git rm --cached file.log
```

再提交一次之后，这个文件就不再被跟踪了。
