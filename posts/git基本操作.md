---
title: Git基本操作
date: 2026-09-13 23:59:01
tags:
---

Git是一个开源的分布式版本控制系统。

> “分布式”是指：每个开发者的电脑上，都保存着一份完整的 Git 仓库，包括文件和全部版本历史。

Git 把内容按元数据方式存储，这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。

>Git 不主要按“文件名”保存内容，而是根据内容计算出一个 SHA-1 哈希值，并用它来标识和查找这份内容。SHA-1 会把任意长度的数据转换成一个固定长度的 40 位十六进制字符串。

### Git配置

Git 提供了一个叫做 git config 的命令，用来配置或读取相应的工作环境变量。这些变量可以存放在以下三个不同的地方：

- `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。
- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

配置用户信息：配置个人的用户名称和电子邮件地址，这是为了在每次提交代码时记录提交者的信息。

```shell
git config --global user.name "runoob"
git config --global user.email test@runoob.com
```

用了 `--global` 选项那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 `--global` 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

```shell
git config --list #显示当前的 git 配置信息

git config -e              # 针对当前仓库编辑 git 配置文件
git config -e --global   # 针对系统上所有仓库
```

### Git工作流程

![img](https://gitee.com/wenswuu/pictures/raw/master/git-command.webp)

Git的工作流程主要分为四个主要部分：

- 工作区（Working Directory）：实际修改文件的地方。
- 暂存区（Staging Area）
- 本地仓库（Local Repository）
- Remote

克隆仓库到指定目录：

```shell
git clone <repo> <directory>
```

新建分支：

```shell
git checkout -b new-feature
```

暂存文件，将修改过的文件添加到暂存区：

```shell
git add filename #添加某个文件
git add . #添加所有文件
```

提交修改，将暂存区的更改提交到本地仓库：

```shell
git commit -m "Add new feature"
```

拉取最新修改，在推送本地更改之前，最好从远程仓库拉取最新的更改，以避免冲突：

```shell
git pull origin main/new-feature
```

推送更改，将本地的提交推送到远程仓库：

```shell
git push origin new-feature
```

创建 Pull Request（PR），在 GitHub 或其他托管平台上创建 Pull Request，邀请团队成员进行代码审查。PR 合并后，你的更改就会合并到主分支。在 PR 审核通过并合并后，可以将远程仓库的主分支合并到本地分支。

删除分支：

```shell
git branch -d new-feature
git push origin --delete new-feature #从远程仓库中删除
```

### Git基本操作

初始化一个Git仓库：Git 使用 `git init` 命令来初始化一个 Git 仓库，Git 仓库会生成一个 .git 目录，该目录包含了资源的所有元数据，其他的项目目录保持不变。

```shell
git init #使用当前目录作为 Git 仓库
git init newrepo #使用指定目录作为Git仓库
```

提交与修改：

| 命令             | 说明                                     |
| :--------------- | :--------------------------------------- |
| `git status`     | 查看仓库当前的状态，显示有变更的文件。   |
| `git diff`       | 比较文件的不同，即暂存区和工作区的差异。 |
| `git range-diff` | 比较两个提交范围之间的差异。             |
| `git reset`      | 回退版本。                               |
| `git rm`         | 将文件从暂存区和工作区中删除。           |
| `git mv`         | 移动或重命名工作区文件。                 |
| `git notes`      | 添加注释。                               |
| `git checkout`   | 分支切换。                               |
| `git switch`     | 更清晰地切换分支。                       |
| `git restore`    | 恢复或撤销文件的更改。                   |
| `git show`       | 显示 Git 对象的详细信息。                |

提交日志：

| 命令               | 说明                                                         |
| :----------------- | :----------------------------------------------------------- |
| `git log`          | 查看历史提交记录                                             |
| `git blame <file>` | 以列表形式查看指定文件的历史修改记录                         |
| `git shortlog`     | 生成简洁的提交日志摘要                                       |
| `git describe`     | 生成一个可读的字符串，该字符串基于 Git 的标签系统来描述当前的提交 |

远程操作：

| 命令            | 说明                        |
| :-------------- | :-------------------------- |
| `git remote`    | 远程仓库操作                |
| `git fetch`     | 从远程获取代码库            |
| `git pull`      | 下载远程代码并合并          |
| `git push`      | 上传远程代码并合并          |
| `git submodule` | 管理包含其他 Git 仓库的项目 |

### Git文件状态

未跟踪（Untracked）： 新创建的文件最初是未跟踪的。它们存在于工作目录中，但没有被 Git 跟踪。

```shell
touch newfile.txt  # 创建一个新文件
git status         # 查看状态，显示 newfile.txt 未跟踪
```

已跟踪（Tracked）： 通过 `git add` 命令将未跟踪的文件添加到暂存区后，文件变为已跟踪状态。

```shell
git add newfile.txt  # 添加文件到暂存区
git status           # 查看状态，显示 newfile.txt 在暂存区
```

已修改（Modified）： 对已跟踪的文件进行更改后，这些更改会显示为已修改状态，但这些更改还未添加到暂存区。

```shell
echo "Hello, World!" > newfile.txt  # 修改文件
git status                          # 查看状态，显示 newfile.txt 已修改
```

已暂存（Staged）： 使用 `git add` 命令将修改过的文件添加到暂存区后，文件进入已暂存状态，等待提交。

```shell
git add newfile.txt  # 添加文件到暂存区
git status           # 查看状态，显示 newfile.txt 已暂存
```

已提交（Committed）： 使用 `git commit` 命令将暂存区的更改提交到本地仓库后，这些更改被记录下来，文件状态返回为已跟踪状态。

```shell
git commit -m "Added newfile.txt"  # 提交更改
git status                         # 查看状态，工作目录干净
```

### Git分支管理

创建分支：

```shell
git branch <branchname>
git checkout -b <branchname>  #创建新分支并立即切换到该分支下
#等价于：git branch <branchname> + git chechout <branchname>
```

查看分支：

```shell
git branch # 查看所有分支
git branch -r #查看远程分支
git branch -a #查看所有本地和远程分支
```

合并分支：

```shell
git merge <branchname> #将其他分支合并到当前分支
```

解决合并冲突：当合并过程中出现冲突时，Git 会标记冲突文件，你需要手动解决冲突。标记冲突解决完成后再`git add filename`,`git commit`。

删除分支：

```shell
git branch -d <branchname> #删除本地分支
git branch -D <branchname> #强制删除未合并的分支

git push origin --delete <branchname> #删除远程分支
```

切换分支：

```shell
git checkout <branchname>
```

### Git提交历史

Git 提交历史一般常用两个命令：

- `git log` - 查看历史提交记录。
- `git blame <file> `- 以列表形式查看指定文件的历史修改记录。

`git log`命令的基本语法：

```shell
git log [选项] [分支名/提交哈希]
```

常用的选项包括：

- `-p`：显示提交的补丁（具体更改内容）。
- `--oneline`：以简洁的一行格式显示提交信息。
- `--graph`：以图形化方式显示分支和合并历史。
- `--decorate`：显示分支和标签指向的提交。
- `--author=<作者>`：只显示特定作者的提交。
  - `--since=<时间>`：只显示指定时间之后的提交。如`git log --since="2024-01-01"`。
- `--until=<时间>`：只显示指定时间之前的提交。
- `--grep=<模式>`：只显示包含指定模式的提交消息。
- `--no-merges`：不显示合并提交。
- `--stat`：显示简略统计信息，包括修改的文件和行数。
- `--abbrev-commit`：使用短提交哈希值。
- `--pretty=<格式>`：使用自定义的提交信息显示格式。

git blame 命令格式如下：

```shell
git blame [选项] <文件路径>
```

常用的选项包括：

- `-L <起始行号>,<结束行号>`：只显示指定行号范围内的代码注释。
- `-C`：对于重命名或拷贝的代码行，也进行代码行溯源。
- `-M`：对于移动的代码行，也进行代码行溯源。
- `-C -C` 或 `-M -M`：对于较多改动的代码行，进行更进一步的溯源。
- `--show-stats`：显示包含每个作者的行数统计信息。

### 恢复和回退

1.git checkout

git checkout 命令用于切换分支或恢复工作目录中的文件到指定的提交。

恢复工作目录中的文件到某个提交：

```shell
git checkout <commit> -- <filename>
```

切换到特定提交：

```shell
git checkout <commit>
```

2.git reset

git reset ：重置当前分支到特定提交，可以更改当前分支的提交历史，它有三种主要模式：--soft、--mixed 和 --hard。

- --soft：只重置 HEAD 到指定的提交，暂存区和工作目录保持不变。

  ```shell
  git reset --soft <commit>
  ```

- --mixed（默认）：重置 HEAD 到指定的提交，暂存区重置，但工作目录保持不变。

  ```shell
  git reset --mixed <commit>
  ```

- --hard：重置 HEAD 到指定的提交，暂存区和工作目录都重置。

  ```shell
  git reset --hard <commit>
  ```

3.git revert

用于撤销某次提交，git revert 命令创建一个新的提交，用来撤销指定的提交，它不会改变提交历史，适用于已经推送到远程仓库的提交。

```shell
git revert <commit>
```

4.git reflog

用于查看历史操作记录，git reflog 命令记录了所有 HEAD 的移动。即使提交被删除或重置，也可以通过 reflog 找回。

```shell
git reflog
```

利用 reflog 可以找到之前的提交哈希，从而恢复到特定状态。例如：

```shell
git reset --hard HEAD@{3}
```

