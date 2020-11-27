#### 一、基本使用

##### 首先配置用户信息

git config  --global user.name   NinthMonee --replace-all

git config --global user.email  xxx@qq.com --replace-all

##### 生成秘钥

ssh-keygen -t rsa -C   xxx@qq.com

##### 配置别名(可选)

git config --global alias.st  status    ----这是为git status配置一个全局的别名

##### 获取状态

git status / 使用别名：git st

##### 初始化工作区

git  init

##### 提交工作区到暂存区

git add .

##### 提交暂存区到本地仓库

git commit -m "提交的描述信息"

##### 提交本地仓库到远程仓库

###### `如果没有分支`

​	 首先需要建立自己的一个分支，并切换到自己的分支。

​		git branch ty

​		git checkout ty

​		或者可以简写为：

​		git checkout -v ty

然后，提交到远程仓库的时候，需要先设置在远程仓库创建一个远程分支。

git push	--set-upstream origin master



  git push -u origin ty

###### `如果已经有分支`

那么直接提交到自己的分支即可

git push



列出所有分支

git branch -a

列出本地分支

git branch

列出所有远程分支

git branch -r

