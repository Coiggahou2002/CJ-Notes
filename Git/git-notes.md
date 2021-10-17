# 与 GitHub 账号联合使用

**原理：**

本地用 Shell 生成一个 `SSH key`，然后在 GitHub 个人账户设置页面将该 `SSH key` 添加进去，以后 `pull` 或者 `push` 都会带上 `SSH key` ，GitHub 官网通过这个 `SSH key` 来辨认你的身份.

# 常用命令

1.`git init` 

在某个目录下执行此命令，将该目录初始化为一个仓库。

如果是直接从远程克隆仓库，那么准备一个空目录然后在里面 clone 就好，不需要 init.  

2.`git clone <项目地址>`

在某目录下执行该命令，就能将项目克隆到本地

3.`git add <文件名>`

在本地仓库修改完某个文件后，需要用这个命令让修改生效

4.`git commit -m "提交修改的留言"`

在完成 `add` 以后需要执行 `commit` ，表示提交一次，在留言处对所作的修改进行概述.

5.`git pull <远程仓库名> <远程分支名>:<本地分支名>`

此命令用于取回远程仓库的变化，**并与本地分支合并**，远程仓库名一般是 `origin` ，拉取的分支一般是 `main`，至于拉到本地的什么分支，可以自己决定.

6.`git push <远程仓库名> <本地分支名>:<远程分支名>`

将自己的某个本地分支推到远程的某个分支，一般推完之后在 GitHub 上可以提交 `pull request`，别人 `review` 你提交的代码，如果他 `approve` 你的修改，那么你的 `pull request` 就可以通过，通过之后，拥有被合并进主分支的权利.

7.`git checkout <分支名>`

在本地已有的分支之间切换

8.`git branch`

查看本地已有的分支、以及当前处于哪个分支

9.`git branch <新分支名>`

在本地创建一个新分支，但仍留在原分支，不切换到该分支

10.`git checkout -b <新分支名>`

在本地创建一个新分支，并切换到该分支.

# 事件

在本地创建一个仓库，推到远程的空仓库

```shell
cd <local_repo_dir>
git init
git add .
git commit -m "commit_message"
git remote add origin <remote_repo_url>
git push origin main
```
