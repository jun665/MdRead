# Git笔记666

本文档由`帅比闻`编辑，如果你不想掌握基础知识，你可以在文档底部`实际操作`快速使用。

#### 关于Git

**什么是Git？**

​		Git是linus用c花费两周写的分布式版本控制系统。

**什么是分布式版本控制？**

​		你有一个开源项目，很多人共同开发，然后源码提交到一起，如果人为整理，不就麻烦了。于是出现了一个版本管理系统，可帮你完成各种麻烦操作。

**分布式版本控制系统：**开源项目存放在本机仓库，其他开发者可以通过提交修改内容进行版本更新。

#### 安装Git

**1.Linux安装：**

检查是否已安装：输入git命令，提示如下则表示未安装。

```shell
$ git
The program 'git' is currently not installed. You can install it by typing:
$ sudo apt-get install git		#安装方法
```

**2.Ubuntu/Debian	Linux安装：**

输入下面的命令即可安装：

```shell
#正常版本
$ sudo apt-get install git
#老旧版本
$ sudo apt-get install git-core
```

**3.其他Linux源码安装:**

先从Git官网下载源码并解压，然后依次输入如下命令：

```shell
$ ./config
$ make
$ sudo make install
```

**4.Windows安装：**

从[Git官网](https://git-scm.com/downloads)下载安装程序，默认选项安装，在开始菜单找到"Git"->"Git Bash",弹出控制台即可！

安装完毕后需要设置仓库名字、邮箱，因为每个机器需要介绍自己：

```shell
#--global参数本机表示所有Git仓库
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

#### ----------

#### 本地仓库

版本库又名仓库，英文名**repository**，可以实际上一个目录，该目录被Git管理，内部文件内容的变动都会被跟踪，并可以在之后还原。

##### 创建本地库

进入一个目录，并使用如下命令即可(生成的.git目录使用**ls -ah**可视)：

```shell
#新键版本库，创建目录用mkdir 目录
$ git init
```

**暂存区：**

stage（或者叫index）：工作区每次修改都得添加到暂存区，否则可能会提交上次添加到暂存区的文件。

![](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

**Windows说明：**仓库目录不要用中文、MS-Word不会被跟踪、自带记事本不要用，推荐Notepad++设置UTF-8 without BOM

##### 文件添加

先将移到Git仓库目录内，子目录也行，然后执行添加操作（实际上是提交一次操作）：

```shell
#添加文件到暂存区，文件名为.时表示全部文件
$ git add 文件名
#提交到本地仓库
$ git commit -m "提交说明"
#推送到远程仓库
$ git push
```

##### 仓库状态

查看仓库的状态(是否被修改等)：

```shell
#查看文件状态
$ git status
#查看最后一次修改的内容
$ git diff 文件名
```

##### 版本回退

commit：快照。

HEAD：当前版本，后面每加一个^就表示再上一代，也可用HEAD~n表示上n代。

```shell
#查看提交历史：添加--pretty=oneline参数可以使信息减少
$ git log
#将当前版本回退到上一个版本，窗口关闭前可用版本号回跳
$ git reset --hard HEAD^
#跳到某个版本：版本号为commit的值，可以输任意位，但注意类似的版本号
$ git reset --hard 1094a
```

如果关掉了电脑，想撤销回退，找不到版本id 查看命令记录：

```shell
$ git reflog
```

**修改管理：**

```shell
#查看工作区和版本库里最先版本区别
$ git diff HEAD -- 文件名
```

**撤销修改：**

```shell
#把文件在工作区的修改撤销
$ git checkout -- 文件名
#把文件在暂存区的修改撤销
$ git reset HEAD 文件名
```

如果没添加到暂存区，就撤销工作区的修改；如果没提交暂存区，就撤销暂存区的修改；如果没提交到远程仓库，就回退版本。

 

##### 文件删除

删除了工作区文件后，工作区和版本库内容不一致，此时无法推送，查看状态时Git会提醒你是否删除版本库中对应的文件：

```shell
#删除版本库中的文件，删目录要在目录名前加-r
$ git rm 文件名
#撤销删除
$ git restore 文件名
#删错了，版本库文件恢复到工作区
$ git checkout -- 文件名
#add也可以，相当于将删除操作提交
$ git add 文件名
```



#### ----------

#### 远程仓库

一台主机作为服务器，可以克隆仓库到自己电脑，也可以将自己的仓库推送到服务器。

**GitHub:**提供Git仓库托管服务，可以用来创建远程仓库。[地址](https://github.com/)

##### SSH秘钥

> GitHub支持ssh协议，它可以通过公钥来判断推送的文件是由你推送的。
>
> 密钥：一台主机创建一个即可，一次创建一直可用，与仓库无关。

本地Git仓库和GitHub仓库之间的传输通过SSH加密,因此需要设置密钥：

```shell
#切换到ssh
$ cd ~/.ssh
#生成密钥
$ ssh-keygen -t rsa -C "你的邮箱"
```

> 注意：不要起名（id_rsa除外），不要设置密码，因为设置名字后，你的名字与ssh内置名字不一致，导致连接错误。
>
> 在生成密钥的时候，请在 “ ~/.ssh/ ”目录下操作。或者生成后把文件移动到“ ~/.ssh/ ”目录下。

如果非要起个性名字：

~~~shell
#家目录 .ssh 里面添加 config 文件（不要后缀）
#配置路径和私钥的路径
Host github.com
  HostName github.com
  IdentityFile ~/.ssh/密钥文件名

~~~

查看加入的密钥列表：

```shell
$ ssh-add -l
```

2.登陆GitHub，打开https://github.com/settings/keys页面添加密钥文件xxx.pub内容（多台机器，添加多个）。

3.验证是否可用：

```shell
$ ssh -T git@github.com
```

##### 创建远程库

登录GitHub创建一个仓库即可。

> README.md`文件：创建时勾选勾选`Initialize this repository with a README`即可。
>
> 创建完毕后，GitHub会提示可以从此仓库克隆出新仓库，把本地仓库的内容推送到GitHub仓库。



##### 关联远程库

> 后面部分可在仓库下的Code>SSH找到。
>

```shell
#关联远程仓库：origin是默认的远程库名
$ git remote add origin git@github.com:GitHub账户/远程仓库名.git
```

##### 文件推送

> 推送本地库内容到远程库：
>

```shell
#首次推送，推送整库（需要-u参数），末尾加-f强推
$ git push -u origin master
#正常推送，推送master分支
$ git push
```

git push命令实际上推送master分支，因为第一次仓库为空，所以要将分支信息也推送上去，因此新远程库推送需要加入-u参数。

> SSH警告：SSH连接在首次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入`yes`回车即可。
>

##### 文件拉取

> 注意：要在本地创建的版本库中拉取

```shell
#拉取origin/dev最新推送
$ git pull
```



##### 仓库下载

注意：实际上就是把整个仓库下载到本地，最好不要在仓库里拉取。

```shell
#将远程库内容克隆到本地库
$ git clone git@github.com:GitHub账户/远程仓库名.git
#直接通过https下载（会弹出登录窗口）
$ git clone https://github.com/用户名/仓库名.git
```

> Git支持多种协议，包括`https`，但`ssh`协议速度最快。
>



#### 分支管理

分支就像鸣人的影分身，分身之间信息互不影响，分身回到本体后，信息就统一起来了。

![](https://www.liaoxuefeng.com/files/attachments/919021987875136/0)

你开发一个新功能，开发一半时，提交会使代码库不完整，从而影响其它开发者，不提交又有数据丢失的风险。

因此你可以创建一个自己专属分支，将数据提交到分支上，开发完毕后再合并到统一分支上。

> **分支实现原理**：https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424

**创建分支（dev）：**

```shell
#git checkout命令加上-b参数表示创建并切换
#等价于 $ git branch [分支名] 和 $ git checkout [分支名]
#方法1
$ git checkout -b 分支名

#方法2
$ git switch -c 分支名
```

**查看分支：**

```shell
#输出分支列表，*号标记的是当前分支
$ git branch
```

**切换分支：**

```shell
#方法一
$ git checkout 分支名

#方法二
$ git switch 分支名
```

**合并分支：**和主分支合并：将当前分支切换到master、再合并私人分支。

```shell
#合并指定分支到当前分支
$ git merge 分支名

#--no-ff参数，表示禁用Fast forward，-m表示添加说明
$ git merge --no-ff -m "说明" 分支名
```

​		如果主分支和私人分支相关文件都发生了更新，需要手动合并解决冲突，查看文件状态（git status）并修改即可。（用`git log --graph`命令可以看到分支合并图。）

Fast forward：删除分支后，丢掉分支信息（即没有分支历史记录）。

**删除分支：**

```shell
#删除（合并前无法删除）
$ git branch -d 分支名
#强行删除（随时可以删除）
$ git branch -D 分支名
```



**Bug分支：**

工作进行一半，你需要创建分区修复一个紧急bug，但当前工作未开发完而无法提交，所以需要将当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```shell
#储藏当前工作现场，并可恢复
$ git stash
```

查看工作现场存储列表：

```shell
$ git stash list
```

恢复工作现场：

```shell
#恢复但不删除存储的现场
$ git stash apply
#恢复并删除存储的现场
$ git stash pop
```

复制特定提交到当前分支：

如果一个bug在多个分支都存在，我们可以在当前分支修改bug并提交，然后切到其它分支上复制该提交所做的修改，省去多次修改的麻烦。

```shell
$ git cherry-pick 4c805e2
```

**Feature分支：**

开发过程中会不断添加功能，最好每添加一个新功能就创建一个分支Feature分支，完成功能分支任务后，再合并到主分支上。

如果分支开发完毕，但临时不需要此分支，我们需要强行删除:

```shell
$ git branch -D 分支名
```



##### 多人协作

| 指令                   | 用途                                         |
| ---------------------- | -------------------------------------------- |
| git remote             | 查看远程库信息，加-v显示详细（显示推送地址） |
| git push origin 分支名 | 推送到分支，master为主分支                   |
|                        |                                              |

你的小伙伴克隆一个远程库上的master，此时你已经创建了一个dev分支，他想在dev上开发，需要创建远程dev到本地，他就可以随意在dev上推送了

```shell
#创建远程dev到本地
$ git checkout -b dev origin/dev
```

如果你在他提交前已经对dev做了一次提交，他再提交，就会产生冲突，此时他需要将dev上的最新提交拉到本地，然后在本地合并，（如果冲突，手动修改）再提交。

> 需要设置本地dev与远程dev分支的链接，否则无法拉取。

```shell
#关联远程dev和本地dev分支
$ git branch --set-upstream-to=origin/dev dev
#拉取origin/dev最新提交
$ git pull
```

**Rebase：**

必须得`git pull`后合并再推送，此时提交的历史就会出现一个分叉...靠哇！后推送的小伙子再看提交历史不就辣住胯了？

如何让这个分叉变成直线：推送之后，使用命令`git rebase`即可。

#### 标签管理

发布一个版本时，我们通常先在版本库为该版本中打一个唯一标签（tag），可通过标签来获取该版本。（提交的时候，不用再输入版本ID，输入标签即可）

**创建/查看标签：**切换到该分支，使用如下命令打标签。

```shell
#为当前分支打标签v1.0
$ git tag v1.0
#查看当前分支所有标签（不按提交时间排序，按标签名排序）
$ git tag
#根据指定标签查看版本
$ git show v0.9
```

如果忘了给之前提交打标签，则可以补上：

```shell
#查看提交历史
$ git log --pretty=oneline --abbrev-commit
#打标签（版本Id前可以添加-m参数添加说明）
$ git tag v0.9 1094adb
```

**删除/推送标签：**

```shell
#删除本地标签
$ git tag -d v0.1
#删除远程标签(先删除本地)
$ git push origin :refs/tags/v0.9
#推送指定标签到远程
$ git push origin v1.0
#推送全部未推送的标签到远程
$ git push origin --tags
```



------------

### 其它

#### 代码托管

GitHub：https://www.liaoxuefeng.com/wiki/896043488029600/900937935629664

Gitee：https://www.liaoxuefeng.com/wiki/896043488029600/1163625339727712

#### 自定义Git

| 指令                                | 用途          |
| ----------------------------------- | ------------- |
| $ git config --global color.ui true | 让Git显示颜色 |

#### 文件排除

在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后填入忽略的文件名（可以下载一个标准文件，再添加自己的即可）。

内容示例：https://github.com/github/gitignore 提供一些自动生成的范例。

```shell
# Windows:如winddows自动生成的文件
Thumbs.db
ehthumbs.db
Desktop.ini
#加入自己定义的排除文件
a.txt
b.md
```

**windows注意**：资源管理器里新建会提醒你输入文件名，可以在编辑器**另存为**解决。

如果想添加已经被忽略的文件：

```shell
#强行加入
$ git add -f 文件名
#查看忽略位置，然后修改即可，查看指令如下：
$ git check-ignore -v 文件名
```

#### 配置别名

指令单词太长、太难记辣住胯？起个别名直接用别名就行了：

```shell
#--global表示全局设置，所有仓库可用，默认当前仓库
$ git config --global alias.新词 旧词
```

> 配置文件在.git/config中的[alias]后面，可删除。



#### Git服务器

GitHub就是一个免费托管开源代码的远程仓库，但有时候不想公开，但又不想交钱，此时需要自己搭建远程仓库。

> 搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

1.安装Git：

```shell
$ sudo apt-get install git
```

2.创建一个`git`用户，用来运行`git`服务：

```shell
$ sudo adduser git
```

3.创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，一行一个。

4.初始化Git仓库：

创建一个目录，在该目录下创建裸仓库，裸仓库没有工作区。

| 指令                               | 用途             |
| ---------------------------------- | ---------------- |
| $ sudo git init --bare 仓库名.git  | 创建一个裸仓库   |
| $ sudo chown -R git:git 仓库名.git | 把owner改为`git` |
|                                    |                  |

5.禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell（可以正常通过ssh使用git），这可以通过编辑`/etc/passwd`文件完成。

```shell
$ git:x:1001:1001:,,,:/home/git:/bin/bash
#改为
$ git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

6.克隆远程仓库：

```shell
$ git clone git@服务地址:/仓库目录/仓库名.git
```

管理公钥：

人少：直接添加到/home/git/.ssh/authorized_keys文件；人多：用[Gitosis](https://github.com/res0nat0r/gitosis)。

#### GitGUI

如果不想用指令操作，旧直接怼桌面应用了Github桌面、[SourceTree](https://www.sourcetreeapp.com/)，当然推荐你熟练掌握指令。

### 实际操作

#### 创建+推送

> 创建本地/远程库、关联、拉取/推送

**1.创建密钥**

**2.创建主目录（可忽略）**

创建一个目录：作为本地库主目录，创建完毕后打开目录（可不创）。

**3.创建版本库**

主目录创建一个子目录，作为一个本地仓库，进入目录使用`git init`初始化。

**4.创建远程库**

登录Github，创建一个仓库，名称与本地库一致。

**5.本地库关联远程库**

`$ git remote add origin git@github.com:GitHub账户/远程仓库名.git`

**6.编辑本地库并推送**

本地库写一些内容，将文件添加之后，推送到远程库 ：`git push -u origin master `，末尾加`-f`强行覆盖远程库。

#### 每次修改后

> git status查看进度、git log查看提交记录

```shell
#添加文件到暂存区
$ git add .
#提交到本地仓库
$ git commit -m "提交说明"
#推送到远程仓库
$ git push
```

