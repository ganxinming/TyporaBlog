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