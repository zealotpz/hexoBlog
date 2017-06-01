title: Git 常用命令
date: 2016-11-11 11:11:11

tags: Git

desc: 个人Git 常用命令
---

**1.版本回退:**
```bash
git log 查看哈希值
git reset --hard HEAD^git reset --hard 76d4f48c5bc26033fe581e923bbca32e12c8af0b
```

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

<!--more-->
**2.本地与远端差异比较:**

```bash
git fetch origin  git diff master origin/master
```
**3.分支**

查看分支：`git branch`

创建分支：`git branch `

切换分支：`git checkout `

创建+切换分支：`git checkout -b     `

合并某分支到当前分支：`git merge `

删除分支：`git branch -d `

```bash
$ git push origin test:master    // 提交本地test分支作为远程的master分支
$ git push origin test:test      // 提交本地test分支作为远程的test分支
```

```bash
拉取远程仓库：$ git pull [remoteName] [localBranchName] git pull origin demo
```

**4.标签**

4.1 命令`git tag `就可以打一个新标签：

```bash
$ git tag v1.0
```

`git tag -a  -m "blablabla..."`可以指定标签信息；

4.2 命令`git tag`查看所有标签：

```bash
$ git tag
v1.0
```

4.3 推送标签 `git push origin [tagname]`

一次推送所有标签: `$ git push origin --tags`

**5.添加远程库**
```bash
git remote add origin git@server-name:path/repo-name.git；
```
添加完成后,把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了`-u`参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
