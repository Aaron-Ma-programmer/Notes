# Git、Github学习笔记

## 一、Git



### 1、配置git并初始化

> ```c++
> git config --global user.name "Aaron-Ma-programmer" //配置用户名
> git config --global user.email "m13937478568@163.com" //配置邮箱
>     
> git init  //初始化，会在当前文件夹下创建一个隐藏文件  .git  
> ```
>
> 生成的 .git 文件夹会保存我们文件的每个git版本记录和变化

### 2、把文件加进git版本控制系统中

> 初始化之后其实文件并还没有被记录，我们要使用命令git add把文件加进git版本控制系统中
>
> ```c++
> git add test.txt  //添加test.txt到git版本控制系统中
> git add .  //如果文件过多，可以用一个“.” 把当前目录下的所有文件都加入git版本控制系统中
> 
> //此时git只是暂时保存，还不能保存提交记录
> git commit  //可以把刚刚暂时保存的文件提交固定成一个版本
> ```
>
> 回车之后，会弹出vim编译器，让你提交说明。编译后会显示一个文件改变了n行新增，git是按行对比文件的不同（两种情况：新增、删除）

### 3、查看提交信息

> ```c++
> git log  //可查看日志信息
> ```
>
> <img src="https://aaron-typora.oss-cn-beijing.aliyuncs.com/1702212220481.jpg" alt="1702212220481" style="zoom: 80%;" />

>
>
>在这里
>
>+ commit 后面的一段字符：就是这次提交的随机ID作为唯一标识。
>+ Author 就是刚刚配置的用户信息
>+ Date 提交的日期
>+ “第一次提交”   就是刚刚写的提交说明

### 4、修改文件

> 将原来文件中的v0.1 改为 v0.2后：     //新增文件限时绿色，修改文件限时橙色
>
> ```c++
> git add .
> git commot -m "第二次提交"   //写提交说明的简化版，可以直接跳过vim编辑
> git log  //最新的应该在上面
> ```
>
> ![image-20231210212526942](https://aaron-typora.oss-cn-beijing.aliyuncs.com/image-20231210212526942.png)

> 再次修改，将文本中的v0.2 改为v0.3：
>
> ```c++
> git add .
> git commit -m "fix(test): change content"  		//此处是规范了一下提交说明，风格规范不是硬性要求，可以让其他人一目了然你的版本修改内容。  
> ```
>
> 也可以直接使用可视化界面操作。

### 5、回退版本

> ```c++
> git reset --hard debed045bfdd1b9623dlef649ab4e520270082dd  //后面一大段字符是需要回退版本的commit ID     --hard 是重置模式（hard表示硬重置，即覆盖所有变更）
> git log  //此时会发现该版本之后的日志文件也不存在了
> ```
>
> 如果想要实现不同版本之间自由切换可以使用 branch

### 6、Git分支管理

> 分支其实就是把当前版本复制一份
>
> ```c++
> git branch 0.2		//在coommit第二次的时候创建一个0.2分支
> git branch 0.3		//在coommit第三次的时候创建一个0.3分支
> git branch -a		//列出该仓库中的所有分支
> ```
>
> 第四次还是在主分支上，默认是master分支，所以会出现一下状态：
>
> <img src="https://aaron-typora.oss-cn-beijing.aliyuncs.com/image-20231211151628596.png" align="left" style="zoom:50%;" />
>
> （如果希望后续的文本不围绕图片，可以在图片后添加一个 `<br>` 标签来强制换行：“<br clear="left"/>”）
>
> ```c++
> git checkout 0.2  //切换版本
> ```
>
> <img src="https://aaron-typora.oss-cn-beijing.aliyuncs.com/image-20231211152337645.png" align="left" style="zoom:50%;" />
>
>  branch的作用不仅仅是切换版本。在团队开发的时候，在主分支编写代码的同时也可以在分支上继续编写，互不影响。如果需要分支合并的时候，可以通过`git merge`将其合并。
>
> ```c++
> git merge 0.2   //在其他分支上操作，可将0.2分支合并过去。
> ```
>
> 在团队开发中的作用：
>
> <img src="https://aaron-typora.oss-cn-beijing.aliyuncs.com/image-20231211154803393.png" alt="image-20231211154803393" style="zoom: 10%;" align="left"/>

## 二、Github

### 1、创建新仓库

> 登陆github网站，创建自己的仓库，根据需求创建公开或私有的仓库。此处步骤省略。

### 2、初始化仓库

> 在创建完成仓库后，页面会跳转到Code页面。此处有初始化设置的说明教程。
>
> ```c++
> echo "# Notes" >> README.md
>   git init			
>   git add README.md
>   git commit -m "first commit"		//初始化
>   git branch -M main		//创建一个main分支，并把主分支切换成main分支。
>   git remote add origin git@github.com:Aaron-Ma-programmer/Notes.git		//添加一个远程仓库地址，相当于给这个git项目设置一个需要上传到的网盘地址（github）
>   git push -u origin main		//推送上传到这个网盘地址（github）
> ```
>
> 然后会提示你输入用户名和密码，输入github的邮箱和密码即可

### 3、参与开源项目

> + 首先找到一个开源项目，进入这个项目地址。
>
> + 点击右上角的`Fork` （就是将这个代码库复制到自己的账号中，类似branch）
>
> + 复制自己账号仓库中的`HTTPS`链接或者`SSH`链接。
>
> + 在自己电脑上找一个文件夹，用vscode打开，并新建终端。
>
> + 输入 以下代码，将==自己网盘==上的仓库克隆到自己的本地电脑上。
>
> 	```c++
> 	git clone git@github.com:Aaron-Ma-programmer/Test.git .  //在仓库链接后面需要加一个空格和. 
> 	```
>
> + 可以通过以下命令看到仓库链接。
>
> 	```c++
> 	git remote -v   //此时只能看到自己的仓库链接
> 	```
>
> + 复制源仓库的链接并在本地添加上游代码库
>
> 	```c++
> 	git remote add upstream git@github.com:Source/Test.git
> 	```
>
> + 如果要给源仓库创建新的功能，可以创建分支
>
> 	```c++
> 	git checkout -b a			创建一个'a'的分支，并切换进入'a'分支
> 	```
>
> + 接下来便可以在自己电脑上进行编写新功能。
>
> + 在编写完成以后添加并上传到网盘中：
>
> 	```c++
> 	git add . 
> 	git commit -m "添加一个新功能"
> 	git push -u origin a		//将这个文件推送上传到网盘上
> 	```
>
> 	此时在自己的仓库中已经可以看到这个分支
>
> + 来到源仓库中，点击`Pull requests`（拉取请求，简写’pr‘）
>
> + 进来后点击New pull request
> 	在Comparing changes中
> 		左边 base repository:选择 Source/Test；base：选择 main。
> 	    右边 head repository:选择 Aaron-Ma-programmer/Test；compare：选择 a。
>
> + 如果是绿色able to  merge，就可以编写pr信息并Create pull request，即可提交成功。
> 	如果不是绿色的able to  merge，可能是在写代码的时候源仓库提交了新的commit导致版本不一致。更新本地版本即可：
>
> 	```c++
> 	git fetch upstream   //从上游更新最新代码
> 	git merge upstream/main			//把远程的最新代码合并到自己的分支上
> 	git push
> 	```
>
> 	
>
> + 等待源仓库用户审核并合并进主项目

