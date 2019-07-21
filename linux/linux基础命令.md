linux基础命令

##### # pwd  :当前位置

##### # cal   : 查看当前日历    #date:查看当前确切时间

##### 		--指定查看年月#cal 08 1991

##### #echo :输出指定的话，或变量的值

​		--#echo "i love linux"    -- #echo $JAVA_HOME(输出变量时需要$指定)
​		--i love linux           	 --/root/study/jdk8/jdk1.8.0_171

##### #tty : 显示当前终端

##### #whoami: 显示当前用户

##### #clear :清除显示屏

##### # date -- help       :help命令可以查看帮助，参数等.

##### #whatis date      :  whatis命令用于查看当前命令是什么作用.

##### # man date          :man命令查看更多指定命令详细信息。

##### #info date            :info命令比man更加详细

### 关于文件操作的命令

##### #cd  :进入文件

##### #ls :列举文件 

##### 	-- #ls -l 或者ll 查看文件

##### #mkdir         :创建文件夹

##### #touch	      :创建文件

##### #cp source destination   : 把一个文件复制到另一个目的地

#####  #cp -­r example /tmp/expertslogin/  :把一个文件夹所有复制过来需要加个 -r  (注意如果路径加了/,那么就是根路径开始，如果没加，就是当前目录.)

##### #mv source destination ：移动文件或目录

##### #rm (选项)(参数) 

选项 
-d：直接把欲删除的目录的硬连接数据删除成0，删除该目录； 
-f：强制删除文件或目录； 
-i：删除已有文件或目录之前先询问用户； 
-r或-R：递归处理，将指定目录下的所有文件与子目录一并处理； 
–preserve-root：不对根目录进行递归操作； 

##### #rmdir :删除空目录 。#rm  :删除文件

##### #rm -r  ：每删除一个文件，都会询问，但是文件夹会留下。 #rm -rf 直接递归删除所有（包括文件夹）

##### #file  ：查看文件属性

##### #stat: 查看文件状态

##### #cat : 查看文件所有内容

##### #head :查看文件前面几行内容,默认10行。（用于随便看看文件）

##### #tail -n 4 b.txt  :和head一样，但是这个可以指定查看几行

##### #wc b.txt   (查看文件统计行数，字数) 行数为21、单词数28、字节数86

##### 21 28 86 b.txt

##### #grep s b.txt  在某文件匹配字符串。可以使用正则

##### This is my the second to write Linux commonds.(匹配到含s的字符串)

##### #ln TARGET LINK_NAME  创建一个文件的硬链接(相当于重写了一个)

##### #ln  -s TARGET LINK_NAME 创建一个软链接(相当于一个快捷方式)

##### #vi  :编辑文件，修改文件。

##### #alias c="clear"：为命令起个别名。

##### #w :查看用户进入系统的信息

##### #last |head  :last命令用来查看最近几次有谁登陆，加上管道head用来筛选前10条. 

root     pts/0        117.136.125.75   Tue Mar 19 19:44   still logged in   
root     pts/1        182.97.233.15    Tue Mar 19 15:19 - 17:48  (02:29)    
root     pts/0        117.136.125.75   Tue Mar 19 14:09 - 17:29  (03:19) 

##### #lastb ：查看登陆未成功的用户（可以查看谁入侵了）

##### #du hello/  :查看包含文件哪些

52 hello/HelloApp
4 hello/orb.db/logs
20 hello/orb.db

##### #df  ：查看文件磁盘的使用情况

##### #netstat :查看多有程序运行情况。多需要参数，需要时百度

##### #history:查看历史使用的命令











​	







