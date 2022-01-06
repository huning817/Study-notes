- [Git 学习笔记](#git-学习笔记)
  - [Git 简介](#git-简介)
  - [安装 Git](#安装-git)
    - [1.安装及安装成功说明](#1安装及安装成功说明)
    - [2.配置](#2配置)
  - [创建版本库](#创建版本库)
    - [1.版本库介绍](#1版本库介绍)
    - [2.创建版本库](#2创建版本库)
    - [3.添加文件到版本库](#3添加文件到版本库)
  - [时光穿梭机](#时光穿梭机)
    - [1.版本回退](#1版本回退)
    - [2.工作区和暂存区](#2工作区和暂存区)
    - [3.管理修改](#3管理修改)
    - [4.撤销修改](#4撤销修改)
    - [5.删除文件](#5删除文件)
  - [远程仓库](#远程仓库)
    - [1.添加远程库](#1添加远程库)
    - [2.从远程库克隆](#2从远程库克隆)
  - [分支管理](#分支管理)
    - [1.创建与合并分支](#1创建与合并分支)
    - [2.解决冲突](#2解决冲突)
    - [3.分支管理策略](#3分支管理策略)
    - [4.Bug 分支](#4bug-分支)
    - [5.Feature 分支](#5feature-分支)
    - [6.多人协作](#6多人协作)
    - [7.Rebase](#7rebase)
  - [标签管理](#标签管理)
    - [1.创建标签](#1创建标签)
    - [2.操作标签](#2操作标签)
## Git 学习笔记
> 以下学习笔记基于[廖雪峰的 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600)以及遇到问题的各类搜索总结。

### Git 简介
Git 是一种分布式版本控制系统。可以记录每次文件的改动。
1. Git 的诞生出于偶然，是 Linus ( Linux 的创建者 ) 花费两周的时间用 **C** 写出的。
2. 集中式 vs 分布式
   集中式：版本库集中存放于**中央服务器**，必须**联网**才能工作；
   分布式：每个人均有**完整**的版本库，需要交流时，仅需要互相交换大家的修改即可。

### 安装 Git
#### 1.安装及安装成功说明
   * 在 Windows 系统时，可从 [Git 官网](https://git-scm.com/downloads) 直接下载安装程序，按默认选项安装即可。
   * 安装完成，若可从开始菜单找到 “Git Bash”，界面类似于命令行窗口，即说明安装成功。  
   * 在 Linux 和 Mac OS X 上均有对应的安装方法。
#### 2.配置
   ```git
   $ git config --global user.name "Your Name"
   $ git config --global user.email "email@example.com"
   ```
   扩展：
   * `git config` 命令中的 `--global` 参数代表本台计算机上的所有 Git 仓库都使用此配置，也可单独对某仓库指定不同的用户名和 Email 地址。
   * 查看计算机的 Git 配置
     查看全部配置：  `git config --list` (按 <kbd>Q</kbd> 或 + <kbd>Enter</kbd> 退出)
     查看某项配置： `git config user.name`

### 创建版本库
#### 1.版本库介绍
   版本库，又名仓库 ( repository )，可简单理解为一个目录，该目录中的所有文件均可被 Git 管理，Git 可跟踪每个文件的修改、删除等操作，甚至可以**还原**文件在某个时刻的状态。
#### 2.创建版本库
   ```git
   $ mkdir learngit
   $ cd learngit
   $ pwd
   /Users/michael/learngit
   $ git init
   Initialized empty Git repository in /Users/michael/learngit/.git/
   ```
   注：
   * `pwd` ：用于显示当前目录；
   * `git init` ：将该目录变成 Git 可以管理的仓库；
   * `git init` 命令之后会多出一个 `.git` 的目录，该目录用于 Git 用于跟踪管理版本库，**不要手动修改**，以免破坏 Git 仓库。若没有看到该目录，是因为该目录默认是**隐藏**，用 `ls -ah` 命令可以看到。
#### 3.添加文件到版本库
   ```git
   $ git add <file>
   $ git commit -m "<message>"
   ```
   注：
   * `commit` 一次可以提交多个文件和变动，所以使用时可 `add` 多个文件；
   * 所有的版本控制系统只能跟踪**文本文件**的改动，推荐使用标准的 **UTF-8** 编码的文本，还有千万不要使用 Windows 自带的**记事本**编辑任何文本文件，强推**vscode**，yyds :+1:。
   
### 时光穿梭机
常用命令：
* 查看工作区当前状态：`$ git status`；
* 查看上次修改的内容：`$ git diff`；
* 提交文件：
  ```git
  $ git add <file name>
  $ git commit -m "commit described"
  ```
#### 1.版本回退
* 查看从最近到最远的提交日志：`$ git log`；
  增加 `--pretty=oneline` 参数，显示出的日志仅包括 `commit id` 和提交描述；
* 回退到上一个版本：`$ git reset --hard HEAD^`；
  在 Git 中，用 `HEAD` 表示当前版本，也就是最新提交，上个版本是 `HEAD^`，上上个版本就是 `HEAD^^`，往上100个版本可简写成 `HEAD~100`；
* 回退或**前进**到某一版本：`$ git reset --hard <commit id>`；
* 查看所有的历史命令：`$ git reflog`。
#### 2.工作区和暂存区
* 工作区指的是电脑中能看到的目录。
* 工作区有一个隐藏目录 `.git`，不算工作区，而是 Git 的版本库。
* 在版本库中，有一个暂存区（stage、index），还有 Git 为我们自动创建的第一个分支 `master`，以及指向 `master`的一个指针 `HEAD`。
#### 3.管理修改
* Git 比其他的版本控制系统设计的优秀，原因是：Git 跟踪并管理的是修改，而非文件。
* 注意 `git add` 和 `git commit` 的区别。
#### 4.撤销修改
* 撤销文件在工作区的修改：`$ git checkout -- <file name>`；
  若文件在修改后未被放到暂存区，撤销修改回到版本库中的状态；
  若文件在添加到暂存区后作了修改，撤销修改回到添加到暂存区后的状态。
* 撤销暂存区的修改：`$ git reset HEAD <file name>`
#### 5.删除文件
一般的删除操作是指在文件管理器中删除文件，或者用 `rm` 命令删除。
* 一是从版本库中删除该文件：
  ```git
  $ git rm <file name>
  $ git commit -m "<commit described>"
  ```
  注：先手动删除，然后使用 `$ git rm <file name>` 和 `$ git add <file name>` 的效果是一样的。
* 二是误删，需要恢复，用版本库里的版本替代工作区的版本：
  ```git
  $ git checkout -- <file name>
  ```

### 远程仓库
#### 1.添加远程库
   * 首先在Github上**新建**一个仓库；
     此时的仓库是**空的**，可以从该仓库克隆新的仓库到本地，也可以将一个已有的本地仓库与之关联。
   * **关联**本地仓库和远程库；
     ```git
     $ git remote add origin git@github.com:huning817/Study-notes.git
     ```
     注：`origin` 为远程库的名称，也可改为别的。
     若需要重新变更关联的远程库，则先执行以下命令，再重新关联：
     ```git
     $ git remote rm origin
     ```
   * 将本地库的内容**推送**到远程库上（实际上，是将当前分支 `master` 推送到远程）。
     ```git
     git push -u origin master
     ```
     注：`-u` 参数：将本地的 `master` 分支和远程的 `master` 关联，在之后的 `pull` 和 `push` 时可以简化命令。
   * **SSH警告**
     当你第一次使用 Git 的 `clone` 或者 `push` 命令连接 GitHub 时，会得到一个警告：
     ```
     The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
     RSA key fingerprint is xx.xx.xx.xx.xx.
     Are you sure you want to continue connecting (yes/no)?
     ```
     这是因为 Git 使用 SSH 连接，而 SSH 连接在第一次验证 GitHub 服务器的 Key 时，需要你确认 GitHub 的 Key 的指纹信息是否真的来自 GitHub 的服务器，输入 yes 回车即可。
     Git 会输出一个警告，告诉你已经把 GitHub 的 Key 添加到本机的一个信任列表里了：
     ```
     Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
     ```
     这个警告只会出现一次，后面的操作就不会有任何警告了。
     如果还是担心有人冒充 GitHub 服务器，输入 yes 前可以对照 GitHub 的 RSA Key 的指纹信息是否与 SSH 连接给出的一致。
   * 删除远程库
     ```git
     $ git remote rm <origin name>
     ```
     注：
     * 查看远程库详细的信息：`$ git remote -v`；
     * 此处的“删除”是解除了本地和远程的绑定关系，不是物理上删除了远程库。远程库本身没有任何改动。若要删除，需登录 Github 执行删除。
#### 2.从远程库克隆
   若从零开发，推荐先创建远程库，再从远程库克隆。
   ```git
   $ git clone git@github.com:huning817/Study-notes.git
   ```
   注：
   * Github 给出的地址还有：`https://github.com/huning817/Study-notes.git`。
   * Git 支持多种协议，包括 `https`, 但 `ssh` 协议速度最快。

### 分支管理
#### 1.创建与合并分支
在 Git 中，用 `HEAD` 指向当前分支，用 当前分支指向最新的提交。
* 查看分支：`git branch`；
* 创建分支：`git branch <name>`；
* 切换分支：`git checkout <name>` 或者 `git switch <name>`；
* 创建+切换分支：`git checkout -b <name>` 或者 `git switch -c <name>`；
* 合并某分支到当前分支：`git merge <name>`；
* 删除分支：`git branch -d <name>`。
#### 2.解决冲突
* 当多个分支都有了新的提交，此时 Git 无法执行“快速合并 (fast forward)”，会试图合并各自的修改，但可能会存在冲突。可以通过 `git status` 查看冲突文件，然后查看冲突文件的冲突内容，手动修改之后再提交；
  注：合并操作 (merge) 只对**当前所在分支**产生影响，无论是否存在冲突，合并之后，被合并的分支不会发生变化。
* 查看分支合并图：`git log --graph`。
#### 3.分支管理策略
* 合并分支时，如果可能，Git 会采用 `fast forward` 合并，删除分支后，分支信息也被删除，看不出来曾经做过合并，但是如果加上 `--no-ff` 参数，就代表是普通模式合并，合并后的历史有分支，可以看到曾经做过合并。
  ```git
  $ git merge --no-ff -m "merge with no-ff" dev
  ```
#### 4.Bug 分支
* 当你正在工作，急需修改一个 bug，可以先把当前的工作现场“储藏”；
  “储藏”工作现场：`git stash`；
  查看“储藏”的工作现场列表：`git stash list`；
* 恢复“储藏”的工作现场
  1. 用 `git stash apply` 恢复，不删除 stash 内容，需要用 `git stash drop` 删除；
  2. 用 `git stash pop`，恢复的同时删除 stash 内容。
* 若多次 stash，可恢复指定的 stash：
  ```git
  $ git stash apply stash@{0}
  ```
* 在 master 分支上修复的 bug ，合并到当前 dev 分支(相当于将修改 bug 做的提交“复制”到当前分支，避免重复劳动)：
  ```git
  $ git cherry-pick <commit id>
  ```
#### 5.Feature 分支
* 开发一个新 feature，最好新建一个分支；
* 如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <branch name>` 强行删除。
#### 6.多人协作
* 多人协作的工作模式：
  1. 首先，用 `git push origin <branch name>` 推送自己的修改；
  2. 如果推送失败，则因为远程分支比本地更新，需要先用 `git pull` 试图合并；
  3. 如果合并有冲突，则解决冲突，并在本地提交；
  4. 没有冲突或者解决掉冲突后，再用 `git push origin <branch name>` 推送就能成功！
  5. 如果 `git pull` 提示 `no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令 **`git branch --set-upstream-to <branch-name> origin/<branch-name>`**。
* 本地新建的分支如果不推送到远程，对其他人就是不可见的；
* 在本地**创建和远程分支对应的分支**，使用 **`git checkout -b branch-name origin/branch-name`**，本地和远程分支的名称最好一致。
#### 7.Rebase
* rebase操作可以把本地未push的分叉提交历史整理成直线；
  ```git
  $ git rebase
  ```  
* rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。


### 标签管理
Git 的标签是版本库的快照，实质上是**指向某个 commit 的指针**。至于为什么引入 tag，是因为一串乱码的 commit 号在实际使用的时候很不方便，因而产生了容易记住的有意义的 tag。
#### 1.创建标签
   * 首先，切换到需要打标签的分支；
     ```git
     $ git branch
     * dev
       master
     $ git checkout master
     Switched to branch 'master'
     ```
   * 创建标签。
     ```git
     $ git tag v1.0
     ```
   注：
   * 查看所有标签：`$ git tag`；
   * 查看标签信息：`$ git show <tagname>`；
   * 创建带有说明的标签，用 `-a` 指定标签名，`-m` 指定说明文字：
     ```git
     $ git tag -a v0.1 -m "version 0.1 released" <commit id>
     ```
   * 默认标签是打在最新提交的 commit 上，若想打在之前提交的 commit 上，则
     ```git
     $ git tag v0.9 <commit id>
     ```
   * 因标签总是和某个 commit 挂钩。若这个 commit 既出现在 master 分支，又出现在 dev 分支，那么在这两个分支上都可以看到这个标签。
#### 2.操作标签
   * 删除本地标签（创建的标签都只存储在本地，不会自动推送到远程，所以，打错的标签可以在本地安全删除）：
     ```git
     $ git tag -d v0.9
     ```
   * 推送标签到远程（分为推送某个标签和一次性推送全部尚未推送到远程的本地标签两种情况）：
     ```git
     $ git push origin v0.9
     $ git push origin --tags
     ```
   * 删除远程标签（先从本地删除，再从远程删除）：
     ```git
     $ git tag -d v0.9
     $ git push origin :refs/tags/v0.9
     ```
     注：若需要确认是否从远程库删除了标签，可登录 Github 查看。