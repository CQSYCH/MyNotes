

##1、					Git命令

#### 1、关闭远程关联

​	$ git remote rm origin

#### 2、建立远程关联

​	$ git remote add origin git@github.com:CQSYCH/MyNotes.git

#### 3、创建文件

​	$ git touch newFileName.txt

#### 4、创建文件夹

​	$ git mkdir newDirectory(目录)

#### 5、初始化本地仓库

​	$ git init

#### 6、查看状态

​	$ git status

####7、文件添加到暂存区(缓存区)	

​	$ git add newFile.txt(文件名)

#### 8、文件从暂存区移除(缓存区)

​	$ git rm --cached

#### 9、提交Git请求

​	$ git commit -m 'update file'

#### 10、查看日志

​	$ git log

#### 11、查看分支

​	$ git branch

#### 12、创建分支

​	$ git branch development

#### 13、切换分支（切换分支、切换版本tag、切换提交commit、还原文件）

​	**注意：**checkout撤销，只能撤销没有add进暂存区的文件

​	$ git checkout development

#### 14、创建分支并切换到分支

​	$ git checkout -b development

#### 15、合并分支

	##### 	1、先切换到主分支再合并分支

#####	2、$ git merge development

​	**注意：**$ git rebase development,该命令也是合并分支的意思

#### 16、合并后删除分支

​	$ git branch -d development

​       **development 分支的代码已经顺利合并到 master 分支来了，那么development分支没用了，需要删除** 

####17、强制删除分支

​	$ git branch -D development

#### 18、查看版本和添加版本

	##### 	1、git tag v1.0(创建版本)

	##### 	2、git tag(查看历史版本)

#### 19、Git Clone远程仓库到本地仓库

​	**注意:** 本地仓库目录直接克隆下载

​	$ git clone git@github.com:CQSYCH/MyNotes.git 

 #### 20、拉取远程仓库文件

​	**注意：**在远程仓库主分支上拉取仓库文件

​	$ git pull origin master

#### 21、提交本地仓库到远程仓库

​	$ git push origin master

#### 22、查看远程仓库

​	$ git remote -v

#### 23、查看Git 提交改动

​	**注意：**

		##### 		1、红色的部分前面有个 - 代表我删除的 

		##### 		2、绿色的部分前面有个 + 代表我增加的 

​		git diff <$id1> <$id2> # 比较两次提交之间的差异 

​		git diff <branch>..<branch> # 在两个分支之间比较 

​		git diff --staged # 比较暂存区和版本库差异 

​	$ git diff

#### 24、暂存代码

​	**注意：**把当前分支所有没有 commit 的代码先暂存起来 	

​	$ git stash list（暂存代码）

​	$ git stash apply （代码还原）

​	$ git stash drop （删除暂存区Stash记录）

​	$ git stash pop （删除最近一条的 stash 记录 ）

​	$ git stash clear（清空）





