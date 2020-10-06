## 创建代码仓库

* 配置身份

`git config --global user.name "senyihui"`

`git config --global user.email "senyihui@gmail.com"`

后面字符串不加的话就是查询

* 进入项目目录

创建代码仓库 ：在这个目录下输入   `git init`

（删除本地仓库只需删除此文件夹就行）

## 提交本地代码

先用`add`添加进来，再使用`commit`真正执行提交操作

添加`build.gradle`文件：`git add build.gradle`

添加所有文件：`git add .`

提交：`git commit -m "Fisrt Commit"`

一定要通过`-m`参数来加上提交的描述信息

## 忽略文件

git提交时会自动忽略`.gitignore`中的文件或者目录

如果有文件或目录不想添加，那么就在`.gitignore`文件中加上

## 查看修改内容

大致文件：`git status`

具体内容：`git diff`后可加具体相对路径

## 撤销未提交的修改

* 未commit：`git checkout filePath`
* commit过：`git reset HEAD filePath`

## 查看提交记录

`git log`

查看具体一条：

`git log 53ec5624ca50a9358a5d91141c7a13787257920b -l`

查看具体一条的修改内容：

`git log 53ec5624ca50a9358a5d91141c7a13787257920b -l -p`

其中减号代表删除的部分，加号代表增加的部分

## 分支的用法

查看分支：`git branch`

创建分支：`git branch version1.0`

切换到分支上：`git checkout version1.0`

合并分支：

`git checkout master`

`git merge  version1.0`

删除分支：`git branch -D version1.0`

## 远程版本库协作

`git remote add origin https://github.com/senyihui/EmailApplication.git`

`git push origin master`

## 版本回退

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

