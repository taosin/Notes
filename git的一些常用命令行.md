# git的一些常用命令行
> 因为日常提交代码一直是用的git，但是一般可视化管理工具用的比较多，而忽视了命令行，而git命令行尤为重要。下面是一些经常用到的git命令行。  

当我在github上创建一个 `repository`  之后，会出现下面的命令：

```bash

$ git init
$ git add README.md
$ git commit -m "first commit"
$ git remote add origin https://github.com/taosin/test.git
$ git push -u origin master

```

那么，上面的这些命令是什么意思呢，接下来，我们将对上面的代码逐行进行解释，那么我们开始进入正题：

#### 1.  第一行：git初始化
``` bash

$ git init
# 将当前目录初始化为Git仓库

```
 关联：

``` bash

$ git init test
# 新建一个文件名为test的文件夹，并将其初始化为Git仓库

```

#### 2.第二行：添加文件到暂存区
```bash

$ git add README.md
# 添加README.md文件到暂存区

```
关联：
```bash

$ git add [file1] [file2] ...
# 添加多个指定文件到暂存区

$ git add [folder]
# 添加指定目录到暂存区，包括子目录

$ git add .
# 添加当前目录的所有文件到暂存区

```

#### 3.第三行：将暂存区的所有文件提交到本地仓库
```bash

$ git commit -m "first commit"
# 提交暂存区的所有文件，first commit 为本次提交的描述

```
关联：
```bash

$ git commit [file1] [file2] -m <message>
# 提交暂存区的指定文件到本地仓库

```

#### 4.第四行：将本地仓库提交到远程仓库
```bash
$ git remote add origin https://github.com/taosin/test.git
```