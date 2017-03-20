# github更新自己Fork的代码

## 主题 GitHub Git

github上有个功能叫fork，可以将别人的工程复制到自己账号下。这个功能很方便，但其有一个缺点是：当源项目更新后，你fork的分支并不会一起更新，需要自己手动去更新。

以gitHub用户:micmiu (账号名)，fork 项目 sql-parser（https://github.com/FoundationDB/sql-parser） 为例子：

### 1、clone 自己账号里fork的分支

git clone https://github.com/micmiu/sql-parser.git

cd sql-parser

### 2、增加远程原始分支到本地（可以用 git remote -v 命令查看远程分支列表）

$ git remote -v
origin	https://github.com/micmiu/sql-parser.git (fetch)

origin	https://github.com/micmiu/sql-parser.git (push)

如果没有远程原始分支则需要增加：

git remote add sql-parser_fdb https://github.com/FoundationDB/sql-parser.git
查看确认远程分支列表：

git remote -v

origin	https://github.com/micmiu/sql-parser.git (fetch)

origin	https://github.com/micmiu/sql-parser.git (push)

sql-parser_fdb	https://github.com/FoundationDB/sql-parser.git (fetch)

sql-parser_fdb	https://github.com/FoundationDB/sql-parser.git (push)

### 3、fetch原始源分支的新版本到本地

git fetch sql-parser_fdb

### 4、合并两个版本的代码

git merge sql-parser_fdb/master

### 5、把最新的代码提交到github自己（micmiu）的账号上

git push origin master
