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



