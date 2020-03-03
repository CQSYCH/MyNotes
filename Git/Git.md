

##1、					Git命令

#### 1、 关闭远程关联

​	$ git remote rm origin

#### 2、建立远程关联

​	$ git remote add origin 'git address'

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

#### 13、切换分支

​	$ git checkout development

#### 14、创建分支并切换到分支

​	$ git checkout -b development

#### 15、合并分支

	##### 	1、先切换到主分支再合并分支

#####	2、$ git merge development

#### 16、合并后删除分支

​	$ git branch -d development

​       **development 分支的代码已经顺利合并到 master 分支来了，那么development分支没用了，需要删除** 

####17、强制删除分支

​	$ git branch -D development

#### 18、查看版本和添加版本

	##### 	1、git tag v1.0(创建版本)

	##### 	2、git tag(查看历史版本)







