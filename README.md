# GitLearn
This repository is for Git learning.  
部分资源来自于课程笔记 https://cn.udacity.com/course/version-control-with-git--ud123  
## 首先配置git
```
git config --global user.name "yourname"
git config --global user.email "example@gmail.com"
```

配置完成之后使用相同的命令查看是否配置成功，只需要将最后的名字和邮箱地址去掉即可。  

也可用下面的命令查看当前配置：
```
git config --list
```

## 建立仓库
在项目目录下输入命令：
```
git init
```

此命令会在根目录下生成一个隐藏的.git文件夹，如果要删除本地仓库，只需要删除这个文件夹(.git/)就行了。

## 提交代码
```
git add example.txt	                //添加example.txt单个文件
git add folder		                //添加folder文件夹下所有文件
git add .		                    //添加所有文件
git commit -m "commit description"	//提交代码

.gitignore                          //忽略文件
```

## 更改最后一个commit
```
git commit --amend
```

## 还原commit
```
git revert <SHA-of-commit-to-revert>
```
git revert 命令用于还原之前创建的 commit,此命令：  
* 将撤消目标 commit 所做出的更改
* 创建一个新的 commit 来记录这一更改

## 重置commit
```
git reset <reference-to-commit>
```
可以用来：
* 将 HEAD 和当前分支指针移到目标 commit
* 清除 commit
* 将 commit 的更改移到暂存区
* 取消暂存 commit 的更改
### git reset 的选项
git 根据所使用选项来判断是清除、暂存之前 commit 的更改，还是取消暂存之前 commit 的更改。这些选项包括：
```
--mixed //取消暂存已被 commit 的更改
--soft  //将 commit 的更改移至暂存区
--hard  //清除commit
```

*一定要谨慎使用 git 的重置功能。这是少数几个可以从仓库中清除 commit 的命令。如果某个 commit 不再存在于仓库中，它所包含的内容也会消失。git 会在完全清除任何内容之前，持续跟踪大约 30 天。要调用这些内容，你需要使用 **git reflog** 命令。*

## 查看更改
```
git status                  //查看修改情况
git diff                    //查看所有更改内容
git diff /src/Test.java		//查看单个文件的更改内容
```

## 撤销更改
```
git checkout	            //撤销所有未提交（未执行add命令）的更改
git checkout /src/Test.java	//撤销单个文件未提交的更改
```
### 对于已经添加（add）的文件(重置)
```
git reset HEAD /src/Test.java	//取消添加
git checkout /src/Test.java	    //再使用checkout撤销更改
```

## 相关commit引用
可以通过以下方式引用之前的 commit：
```
//父 commit – 以下内容表示当前 commit 的父 commit
HEAD^
HEAD~
HEAD~1

//祖父 commit – 以下内容表示当前 commit 的祖父 commit
HEAD^^
HEAD~2

//曾祖父 commit – 以下内容表示当前 commit 的曾祖父 commit
HEAD^^^
HEAD~3
```
^ 和 ~ 的区别主要体现在通过合并而创建的 commit 中。合并 commit 具有两个父级。对于合并 commit，^ 引用用来表示第一个父 commit，而 ^2 表示第二个父 commit。第一个父 commit 是当你运行 git merge 时所处的分支，而第二个父 commit 是被合并的分支。

## 查看历史提交信息（Q键退出）
```
git log		                                //提交记录包括提交id，提交人，
                                            //提交日期和提交描述4个信息

git log --oneline	                        //简略显示

git log --stat	                            //显示 commit 中更改的文件以及添加或删除的行数。
                                            //stat是statistics的简写 

git log -p		                            //该选项是 --patch，可以简写为 -p，
                                            //显示对文件作出实际更改

git log 54214da -1	                        // -1（num not L）只显示一条记录

git log 54214da -1 -p	                    // -p 查看某条提交具体修改的内容

git log --decorate	                        //在 2.13 版中，log 命令已改为自动启用 --decorate 选项。
                                            //这意味不需要在命令中包含 --decorate 选项，它已经自动包含

git log --oneline --decorate --graph --all	//--graph 选项将条目和行添加到输出的最左侧。
                                            //显示了实际的分支。--all 选项会显示仓库中的所有分支。

git show		                            //输出和 git log -p 命令的完全一样
```
## 分支的用法
### 查看当前仓库有那些分支：
```
git branch		//前面带×即为当前分支
```
### 新建分支：
```
git branch newBranch	//新建一个叫newBranch的分支
git checkout newBranch	//切换到newBranch分支

git checkout -b newBranch	//新建分支并切换到分支，相当于执行上面两条命令
```
### 合并分支：
```
git checkout master	//切换到master分支
git merge newBranch	//将newBranch上修改并提交的内容合并到master分支
```
git 中的两种合并：普通合并和快进合并。  
视频讲解：https://s3.cn-north-1.amazonaws.com.cn/u-vid-hd/gQiWicrreJg.mp4 

## 合并冲突指示符解释
编辑器具有以下合并冲突指示符：
```
<<<<<<< HEAD 此行下方的所有内容（直到下个指示符）显示了当前分支上的行
||||||| merged common ancestors 此行下方的所有内容（直到下个指示符）显示了原始行的内容
======= 表示原始行内容的结束位置，之后的所有行（直到下个指示符）是被合并的当前分支上的行的内容
>>>>>>> heading-update 是要被合并的分支（此例中是 heading-update 分支）上的行结束指示符
```
## 解决合并冲突
git 使用合并冲突指示符来告诉你两个不同分支上的哪些行导致了合并冲突，以及原始行是什么。要解决合并冲突，你需要：
* 选择保留哪些行
* 删掉所有带指示符的行
* commit 合并冲突  

删掉所有包含合并冲突指示符的行并选择保留哪个标题后，直接保存文件，并将其添加到暂存区，然后 commit！就像普通合并一样，代码编辑器会弹出，并让你提供 commit 消息。和之前一样，我们经常会使用自动生成的合并 commit 消息，因此在编辑器打开后，直接关闭编辑器并使用自动生成的 commit 消息。

## 删除分支
```
git branch -d newBranch	//删除newBranch分支

git branch -D newBranch	//强制删除。如果某个分支上有任何其他分支上都没有包含的 commit
                        //也就是这个 commit 是要被删除的分支独有的，git不会让你删除该分支，只能强制删除
```

## 添加Tag
```
git tag -a v1.0		    //-a 选项告诉 git 创建一个带注释的标签。如果你没有提供该选项
                        //（即 git tag v1.0），那么它将创建一个轻量级标签。

git tag -a v1.0 a87984	//为之前的提交添加tag

git tag -d v1.0		    //删除tag
```

**HEAD表示当前分支（即活跃分支）**

## 关联到远程库
```
git remote add origin https://github.com/h53/GitLearn.git	//使用https

git pull --rebase origin master	    //获取远程库与本地合并（如果远程库不为空必须做这一步，
                                    //例如有README.md文件存在，否则后面的提交会失败）
```

### 把本地仓库内容推送到远程：
```
git push -u origin master	        //将master分支推送到远程，会要求输入用户名和密码
```

### 将远程版本库下载到本地：
```
git clone https:github.com/example/test.git
```

### 将本地修改推送到远程版本库：
```
git push origin master	//origin 指定远程版本库的Git地址，master 指定同步哪一个分支
```

### 远程版本库同步到本地:
```
git fetch origin	    //同步的代码会存放在origin/master分支上，不会合并到任何分支上
git diff origin/master	//查看远程版本库修改了那些内容
git merge origin/master	//合并到主分支

git pull origin master	//从远程版本库获取代码病合并到本地，相当于fetch和merge放在一起执行
```

## 查看远程仓库
### 方法一
```
git remote show origin
```
显示如下内容
```
* remote origin
  Fetch URL: git@github.com:h53/GitLearn.git
  Push  URL: git@github.com:h53/GitLearn.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
### 方法二
```
git remote -v
```
显示如下内容
```
origin	git@github.com:h53/GitLearn.git (fetch)
origin	git@github.com:h53/GitLearn.git (push)
```

## 生成ssh公钥
SSH公钥默认存储在账户主目录下的.ssh 目录中。  
```
$ cd ~/.ssh  
$ ls  
```  
查看文件夹下的文件是否包含id_rsa和id_rsa.push(或者是id_das和id_das.pub一类成对的文件)，其中有.pub后缀的文件就是公钥，另一个对应的就是私钥。  

如果没有这些文件，甚至连.ssh目录都没有，可以用ssh-keygen来创建。
```
$ ssh-keygen -t rsa -b 4096 -C "邮箱地址"   //-t type -b bit -C comment
```
它先要求你确认保存公钥的位置（.ssh/id_rsa），然后它会让你重复一个密码两次，如果不想在使用公钥的时候输入密码，可以留空。  

当提示你：
```
Your public key has been saved in /home/you/.ssh/id_rsa.pub.
The key fingerprint is: # 03:0e:f2:3b:ca:85:d6:17:a9:7d:f0:68:9d:f0:a2:db "邮箱地址"
```
这个时候，你的本地密钥已经生成了。  
复制 .pub 文件的内容添加到github即可授权成功。  

## 避免每次push都要输入密码
**第一种方式**  
在git clone时使用ssh方式  

**第二种方式**  
如果之前使用的是https方式的git clone，则push仍需要密码，可通过改变remote url的方式
```
git remote rm origin
git remote add origin git@github.com:h53/GitLearn.git
```
然后再使用
```
git push -u origin master
```
就可以推送到远程主机了  

如果出现以下问题：
```
user@localhost:~$ ssh -T git@github.com
sign_and_send_pubkey: signing failed: agent refused operation
git@github.com: Permission denied (publickey).
```
可以使用：
```
ssh-add
```
如果出现以下问题：
```
user@localhost:~$ ssh-add
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0777 for '/home/user/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
可以使用：
```
chmod 0600 /home/user/.ssh/*
```
更改文件权限，然后再运行ssh-add添加，之后的提交等操作不再需要输入passphrase
