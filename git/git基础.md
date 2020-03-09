# git版本仓库

>Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

## 1. git工作流程

![git流程](.\img\git流程.png)

- Workspace：工作区
- Index / Stage：暂存区
- Repository：仓库区（或本地仓库）
- Remote：远程仓库



## 2. Git命令行操作

### 2.1 初始化仓库

```shell
git init
```

### 2.2 设置签名

1. 形式:

   用户名: xfb

   Email地址: xfbliuli@qq.com

2. 作用: 区分不同开发人员的身份
3. 辨析: 这里设置的签名和登录远程库(代码托管中心)的账号. 密码没有任何关系.
4. 命令
   - 项目级别/仓库级别: 仅在当前本地库范围内有效
   
     - `git config user.name tom_pro`
   
     - `git config user.email xfbliuli@qq.com`
   
     - 设置信息将会保存到当前项目 `./git/config` 文件中
   
       ```shell
       [core]
       	repositoryformatversion = 0
       	filemode = false
       	bare = false
       	logallrefupdates = true
       	symlinks = false
       	ignorecase = true
       [user]
       	name = xfb
       	email = noxiue@163.com
       ```
   
   - 系统用户级别: 登录当前操作系统的用户范围
   
     - `git config --global user.name xfb`
   
     - `git config --global user.email xfbliuli@qq.com`
   
     - 信息保存位置: `~/gitconfig` 文件
   
       ```shell
       [user]
               name = youxiue
               email = xfbliuli@qq.com
       [gui]
               recentrepo = D:/Java/ddssoft_work/tysfrz
               recentrepo = D:/Java/ddssoft_work/pboc
               recentrepo = D:/Java/work/mall-swarm
               recentrepo = G:/学习笔记/learn-note
       [http]
               lowSpeedLimit = 0
               lowSpeedTime = 999999
       [difftool "sourcetree"]
               cmd = '' \"$LOCAL\" \"$REMOTE\"
       [mergetool "sourcetree"]
               cmd = "'' "
               trustExitCode = true
       ```
   
       
   
   - 级别优先级
     - 就近原则: 项目级别优先于系统用户级别, 二者都有事采用项目级别的签名
     - 如果只有系统用户级别的签名, 就以系统用户级别的签名为准. 
     - 二者都没有不允许
   



### 2.3 添加提交和查看状态

1. 查看工作区,暂存区 状态命令: `git status`
2. 添加到暂存区的命令: `git add [fileName]`
   - 添加全部: `git add .`   
3. 移除暂存区的命令: `git rm -cached [fileName]`
4. 从暂存区提交到本地库: 
   - `git commit [fileName]` , 然后会进入提交信息描述编辑页面
   - `git commit -m "提交信息描述" [fileName]`



### 2.4 版本的前进后退

#### 2.4.1 查看提交日志 

```shell
# 查看提交日志
git log
# 以漂亮的一行格式查看提交日志
git log --pretty=oneline
# 以更简洁的方式查看日志
git log --oneLine

# 查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）
# 其中 HEAD@{1} 花括号中表示移动到该版本需要移动多少步
git reflog
```

![image-20200302171516071](E:\learn\learn-note\git\img\image-20200302171516071.png)

#### 2.4.2 前进后退

> 本质: 就是指针 **HEAD** 的前进后退

- 基于索引值操作[推荐]
  - `git reset --hard [局部索引值]`  索引值即是log日志第一列那一串字符
  - 例: `git reset --hard dfbf43c`
- 基于 ^ 符号: 只能后退
  - 往后退一步: `git reset --hard HEAD^` 
  - 往后退两步: `git reset --hard HEAD^^`
  - 往后退几步 就有几个 **^**
- 基于 ~ 符号:只能后退
  - `git reset --hard HEAD~n`
  - n是几就退几步

#### 2.4.3 reset 命令的三个参数对比

- --soft 参数
  - 仅仅在本地库移动 HEAD 指针
- --mixed 参数
  - 在本地库移动 HEAD指针
  - 重置暂存区
- --hard 参数
  - 在本地库移动 HEAD 指针
  - 重置暂存区
  - 重置工作区

#### 2.4.4 删除文件并找回

- 前提: 删除前, 文件存在时的状态提交到了本地库.
- 操作: `git reset --hard [局部索引值]`
  - 删除操作已经提交到本地库：指针位置指向上一条历史记录
  - 删除操作尚未 提交到本地库， 指针位置使用 HEAD

#### 2.4.5 比较文件差异

- `git diff [文件名]`
  - 将工作区的文件和暂存区中的文件进行比较

- `git diff [本地库中历史版本] [文件名]`
  - 将工作区中的文件和本地库历史记录比较

- `git diff HEAD`
  - 不带文件名比较，则比较工作区中的所有文件



### 2.5 分支

>在版本控制中, 使用多条线同时推进多个任务, 提高开发效率
>
>各个分支在开发过程中, 如果某一个分支开发失败,不会对其他分支有任何影响, 失败的分支删除重新开始即可. 



分支操作:

1. 创建分支
   
- git branch [分支名]
  
2. 查看分支
   
- git branch -v
  
3. 切换分支
   
- git checkout [分支名]
   - git switch [分支名]
   
4. 创建并切换分支

   - `git checkout -b [分支名]`命令加上`-b`参数表示创建并切换, 相当于执行了 创建命令和切换命令
- `git switch -c [分支名]`
  
5. 合并分支

   - 第一步: 切换到接收修改的分支( 被合并, 增加新内容 ) 上

     git checkout [被合并的分支]

   - 第二步: 执行 merge 命令

     git merge [存在新内容的分支]

6. 解决冲突

   - 冲突的表现

     ![image-20200303094607727](.\img\冲突表现.png)

   - 冲突的解决
     - 第一步: 编辑文件, 删除特殊符号, 把文件修改到满意的程度
     - 第二步: git add [文件名]
     - 第三步: git commit -m "日志信息"
       - 注意: **此时commit 不能带文件名**

6. 删除分支
   
   - `git branch -d [分支名称]`

## 3. git原理

### 3.1 哈希算法

哈希是一个系列的加密算法, 各个不同的哈希算法虽然加密强度不同,但是有以下几个共同点:

1. 不管输入的数据量有多大, 同一个哈希算法,得到的加密结果长度固定.
2. 哈希算法确定, 输入数据确定,输出数据能够保持不变.
3. 哈希算法确定, 输入的数据有变化, 输出数据一定有变化, 而且变化很大.
4. 哈希算法不可逆

git底层采用的是 SHA-1 算法,哈希算法可以被用来验证文件是否发生变化.git就是靠这种机制从根本上保证数据完整性的.

![image-20200303101605676](.\img\hash.png)



### 3.2 git保存版本的机制

​	git把数据看做是小型文件系统的一组快照. 每次提交更新时 Git 都会对当前的全部文件制作一个快照并保存这个快照的索引. 为了高效, 如果文件没有修改. git 不载重新存储该文件, 而是只保留一个链接指向之前存储的文件. 所以Git 的工作方式可以称之为快照流.

​	![image-20200303112749725](.\img\git快照.png)

提交对象及其父对象形成的链条

![image-20200303112944928](E:\learn\learn-note\git\img\git快照链条.png)



## 4.远程仓库

1. 查看关联远程库信息

   `git remote -v`

2. 建立本地库和远程库的关联

   - `git remote add origin https://github.com/youxiue/learn-note.git` 
   - origin 即是后面地址的别名 

3. 推送到远程库

   - 第一次推送：`git push -u origin master` 
     - 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。
     - 意思是推送到origin地址的 master 分支
- 如果推送到远端 一个没有的分支, 远端将会自动创建该分支 例:`git push -u origin login`
     - 如果远端已经存在该分支, 则需要建立 分支间的联系`git push --set-upstream origin login`
     
   - 后续推送：`git push origin master` 



4. 从远程库克隆
   - 命令：git clone https://github.com/youxiue/learn-note.git 
   - 完整的把远程库下载到本地
   - 创建origin 远程地址别名
   - 初始化本地库

5. 远程库的拉取

   > `git fetch`是将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中。
   >
   > 而`git pull` 则是将远程主机的最新内容拉下来后直接合并，即：`git pull = git fetch + git merge`，这样可能会产生冲突，需要手动解决。

   - `get fetch [远程库地址别名] [远程分支名]`
   - `git merge [远程库地址别名/远程分支名]`
   - git pull
     - 将远程主机的某个分支的更新取回，并与本地指定的分支合并: `git pull <远程主机名> <远程分支名>:<本地分支名>`
     - 远程分支是与当前分支合并: `git pull <远程主机名> <远程分支名>`

6. 协同开发冲突的解决

   要点:

   - 如果不是基于GitHub远程哭的最新版所做的修改, 不能推送, 必须先拉取
   - 拉取下来后如果进入冲突状态, 则按照 分支冲突解决 操作即可.

   