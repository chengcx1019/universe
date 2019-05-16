> 基本包括工作和生活git使用要点和使用场景

## 使用规范

### 注释提交规范

squash

## git用法

diff的起点是两个分支的公共父节点（即commit）

### 基本用法

- 删除远端分支

  - `g push origin :branch`

- 配置remote

  - `git remote add <name> <url>`

- 删除文件

  - `git rm —cached` 从仓库中移除文件，但保留工作区文件即保留在本地是未添加状态
    - `git rm —cached filename`
    - `git rm —cached -r dirname`

- 提交历史
  - `git log -p` 显示每次提交的变更
  - `git shortlog`
  - `git show` 当前版本的提交信息

- 将远程仓库中有而本地仓库没有的所有信息拉取下来，然后存储在本地数据库中

  - `git fetch`

- 从指定分支拉取内容，并立即将内容合并进当前所在分支

  - `git pull`

- 查看文件每一行的变更者

  - `git blame filename`

- 提交

  - **git commit -a -m "message"** 跳过暂存，直接提交

  - git commit --amend -m "message" 修改提交的信息

  - git add file && git commit --amend —no-edit 将文件加入之前的提交中

  - git commit --amend --author "Author Name <Author Email>" 在提交中修改邮箱信息，或者你也可以在不同的项目中配置你的邮箱git config user.email “your email id” 

    > 仅在本地仓库使用amend指令

    

- '查看配置信息

  - `git config —list` 列出所有配置信息

- 获取帮助
  - git help <verb>
  - git <verb> --help
  - man git-<verb>

- 取消暂存

  - **git reset HEAD <file>** 

- 取消修改

  - **git checkout -- <file>** 

- git status
  - --short or -s:简洁输出
  - MM:左边的M的表示有修改且此修改已放入暂存区，右边M表示有修改但还未放入暂存区

- git diff
  - --cached:查看暂存区
  - git diff A...B:B分支与A分支的共同分支起，A分支所做的工作

- 文件重命名

  - `git mv` 不可逆操作,一般只有未提交的工作才会彻底丢失

- 合并

  - `git merge --no-commit` 不生成合并提交

### 关键指令详解

- stash

  从远程合并时，本地部分文件里有修改，但远程的相关文件也有修改，但该文件不需要合并远程的修改，同时也希望保留本地的配置，如何合并远程的修改；这个时候就需要使用`stash`指令（储藏与清理），

  ```Shell
  $ git pull
  $ git stash
  $ git pull
  $ git stash pop
  ```

- diff

  查看尚未暂存的文件更新了哪些部分，如果要查看已暂存的内容，则需要添加—cache或者—staged指令，git diff本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动，所以如果暂存了所有的更新过的文件后，再使用git diff后就会什么都没有。	

- rebase

  > rebase与merge的区别在于rebse不会生成额外的提交。使用远端仓库中的最新代码更新本地仓库时，请使用变基。在处理拉取（pull request）请求，以将功能分支和（发布分支或主分支）合并时，请使用合并。
  >
  > 只在本地仓库使用rebase,你不能改变该仓库其他分布（其他对该仓库的clone）与你共有的提交记录信息，否则其他人与主仓库同步则无法识别提交导致混乱。

  1. 开始rebase

     `git checkout new_branch`

     `git rebase master`

     如果没有冲突`new_branch`的第一次提交会加在master提交的后面

  2. 存在冲突，解决冲突后继续rebase

     `git add filename`

     `git rebase --continue`

  3. 如果`new_branch`有多次提交，则下一次提交继续1，2的过程

     

  

- 其他

  fast-forward 在合并时，如果当前分支是需要合并分支的直接上游，合并操作没有需要解决的分歧，git在合并两者的时候，只会简单的将指针向前推进。  

  git checkout --track upstream/feature
  git checkout -b newbranch upstream/feature

  git  push -u(--set-upstream) oragin featureA(local):featureB(server)

### 文件状态

#### 签出文件

- 如果你想取消某些文件在本地的变更，而同时保留另外一些文件在本地的变更，一个比较简单的方法是通过 `git checkout file-name`签出那些你想取消本地的变更的文件。

- 正如前面提到的那样，你也可以从其他分支或者之前的提交中签出文件的不同版本,`git checkout some-branch-name file-name`和 `git checkout {{some-commit-hash}} file-name`。
- 签出的文件处于"暂存并准备提交状态"

#### 文件状态变更

> 包含三种状态的变更

- 本地没有更改，而是从某一次提交commit签出文件:`git checkout commit filename`
  - 签出文件由"已提交状态" -> 处于"暂存并准备提交状态"
- 本地更改执行add添加到"暂存区"，或文件由某一版本签出后，需要回到未暂存状态:`git reset HEAD filename`
  - "暂存状态" -> "未暂存状态"
- 在文件的未暂存状态放弃变更：`git checkout filename`
  - "未暂存状态" -> "原始状态即HEAD版本状态"

### 重置

在git中，有3种类型的重置。重置是让文件回到git历史中的一个特定版本。

1.  `git reset –hard {{some-commit-hash}}` —— 回退到一个特定的历史版本。丢弃这次提交之后的所有变更
2.  `git reset {{some-commit-hash}}` —— 回滚到一个特定的历史版本。将这个版本之后的所有变更移动到**“未暂存”**的阶段。这也就意味着你需要运行 git add . 和 git commit 才能把这些变更提交到仓库
3. `git reset –soft {{some-commit-hash}}` ——回滚到一个特定的历史版本。将这次提交之后所有的变更移动到暂存并准备提交阶段。意味着你只需要运行 git commit 就可以把这些变更提交到仓库

重置的一些用例如下：

1. 如果想清除变更记录，可以使用清理命令——git reset –hard HEAD （最常用）
2. 如果想编辑，以不同的顺序，重新暂存，重新提交文件—— git reset {{some-start-point-hash}}
3. git reset –soft {{some-start-point-hash}}如果想把之前3次的提交，作为一次提交 git reset –soft {{some-start-point-hash}}



### others

两个共同历史的分支进行合并:git pull origin branchname --allow-unrelated-histories