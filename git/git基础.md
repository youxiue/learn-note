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



























