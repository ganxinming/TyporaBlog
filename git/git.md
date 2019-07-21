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



 