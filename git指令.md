# Git

### git init

### git clone

### git status

### git log 

*   git log --online 
*   git log --stat　
*   git log -p 

### git add



### git commit

-   git commit--amend  ：更改最后一个 commit
    -    编辑文件
    -    保存文件
    -    暂存文件
    -    运行 `git commit --amend`

### git diff  

`git diff` 命令用来查看已经执行但是尚未 commit 的更改：

### git tag

### git branch

### git checkout

### git merge

### git revert

### git reset

## 远程仓库

### git remote

要查看当前配置有哪些远程仓库，可以用 `git remote` 命令，也可以加上 `-v` 选项（译注：此为 `--verbose` 的简写，取首字母），显示对应的克隆地址

```
$ git remote -v
origin  https://github.com/johnflame/3dslider_demo.git (fetch)
origin  https://github.com/johnflame/3dslider_demo.git (push)
```

要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 `git remote add [shortname] [url]`：

我们可以通过命令 `git remote show [remote-name]` 查看某个远程仓库的详细信息，比如要看所克隆的 `origin` 仓库，可以运行：

### git push

将本地仓库中的数据推送到远程仓库。实现这个任务的命令很简单： `git push [remote-name] [branch-name]`。

```
$ git push origin master
```

