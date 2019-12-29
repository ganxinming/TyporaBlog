常用命令：

git log :查看日志 

git log  --pretty=oneline :简化日志

git reflog :查看曾使用的命令,

git reset  -- hard HEAD^  : 回退到上一个版本，HEAD代表当前版本,可以写成 HEAD~3 ，回退三个版本

git reset -- hard  [commit id] : 可以通过commit id 来回退版本，commit id不用写全。

##### 详细介绍reset的参数：

git reset  -- soft HEAD^ :将所有已提交的内容恢复到暂存区，本地文件区不受影响。（仅版本库回退一个版本）

git reset  (-- mixed,默认) HEAD^: 将所有已提交的内容和暂存区的内容恢复到上一次版本，本地文件不受影响。(版本库和暂存区恢复上一次版本)

git reset  -- hard HEAD^	：已提交内容回退上一次版本，清空暂存区，本地文件也回退上一次版本。(包括本地工作区，全都回退上一次版本)

git diff HEAD -- test.txt   :查看当前工作区和版本库的区别。

##### 提下三个区间：

工作区：每个人各自写代码的地方。

暂存区：git add 后，代码add后的地方。

版本库：git commit 提交后，提交的地方。

#### 撤销修改：

假如修改文件还在工作区，没有进行add提交，可使用如下命令恢复：

git checkout -- test.txt  （从版本库拉取文件，使工作区恢复和版本库一致）

假如文件已经add，进入暂存区，但是还没有提交，使用如下：

git reset HEAD test.txt   （使暂存区修改撤销，回到未add前，即commit之前，add之后的操作都回退到add之前。）然后，可以再执行下面命令。

git checkout -- test.txt。

### 分支：

创建分支：git branch dev

切换分支：git checkout dev （相当于将当前工作区间从dev复制一份）

或者使用缩写:git checkout  -b dev

查看当前分支：git branch

合并分支：git merge dev （将dev分支合并到当前分支<master>）

删除分支：git branch -d dev

#### 合并分支发生冲突时：

必须手动解决，查看哪些地方有修改的，手动改正。改正add，commit后，自动合并。

查看分支合并树：git log --graph



### 1.上传到github

#### 1.初始化本地仓库

git init 后，git add，git commit后，此时本地仓库完成了，需上传到github。

#### 2.新建一个仓库，初始化不要readme文件。复制仓库地址。

#### 3.创建好Git仓库之后我们就可以和本地仓库进行关联了

git remote add origin https://github.com/guyibang/TEST2.git

#### 4.关联后就可以推送上去了。

git push -u origin master.(第一次需要-u,以后不需要了)

#### 5.如果前面不小心初始化了redme文件，则使用下面命令合并。

git pull --rebase origin master,之后push就可以了。



三、Git命令
查看、添加、提交、删除、找回，重置修改文件

git help <command> # 显示command的help

git show # 显示某次提交的内容 git show $id

git co -- <file> # 抛弃工作区修改

git co . # 抛弃工作区修改

git add <file> # 将工作文件修改提交到本地暂存区

git add . # 将所有修改过的工作文件提交暂存区

git rm <file> # 从版本库中删除文件

git rm <file> --cached # 从版本库中删除文件，但不删除文件

git reset <file> # 从暂存区恢复到工作文件

git reset -- . # 从暂存区恢复到工作文件

git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git ci <file> git ci . git ci -a # 将git add, git rm和git ci等操作都合并在一起做　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　git ci -am "some comments"

git ci --amend # 修改最后一次提交记录

git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象

git revert HEAD # 恢复最后一次提交的状态

查看文件diff

git help <command> # 显示command的help

git show # 显示某次提交的内容 git show $id

git co -- <file> # 抛弃工作区修改

git co . # 抛弃工作区修改

git add <file> # 将工作文件修改提交到本地暂存区

git add . # 将所有修改过的工作文件提交暂存区

git rm <file> # 从版本库中删除文件

git rm <file> --cached # 从版本库中删除文件，但不删除文件

git reset <file> # 从暂存区恢复到工作文件

git reset -- . # 从暂存区恢复到工作文件

git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git ci <file> git ci . git ci -a # 将git add, git rm和git ci等操作都合并在一起做　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　git ci -am "some comments"

git ci --amend # 修改最后一次提交记录

git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象

git revert HEAD # 恢复最后一次提交的状态



查看提交记录

git log git log <file> # 查看该文件每次提交记录

git log -p <file> # 查看每次详细修改内容的diff

git log -p -2 # 查看最近两次详细修改内容的diff

git log --stat #查看提交统计信息