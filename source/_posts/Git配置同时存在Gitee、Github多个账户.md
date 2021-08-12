---
title: Git配置同时存在Gitee、Github多个账户
date: 2021-08-11 22:00:12
# 文章出处名称 #
from: 原创
# 文章出处链接 #
url:
# 文章作者名称 #
author: p0ny
# 文章作者签名 #
about: 一穷二白
# 文章作者头像 #
avatar: 
# 是否开启评论 #
comments: true
# 文章标签 #
tags: Git
# 文章分类 #
categories: Git
# 文章摘要 #
description: Git配置同时存在Gitee、Github多个账户
# 文章置顶 #
sticky: 1
---
# Git配置同时存在Gitee、Github多个账户

### 一、清空 Git 的全局配置文件里面的内容

>1. **清空** Git 的全局配置文件里面的内容
>
>    全局配置文件的路径：   
>    `"C:\Users\用户名\.gitconfig"`
>
>    该文件下有类似如下内容，则**清空**
>    ```
>    [user]
>    email = p0ny233@88.com
>    name = p0ny233
>    ```
>
>    **刚安装 `git` 的可以忽略这一步**
>

### 二、生成新的 SSH keys

>  0. 若在 `C:/Users/用户名/` 目录下没有 `.ssh` 文件夹，不允许创建以 `.` 开头的文件,那么就需要使用命令任意生成
>     >  1. 打开 `git` 终端
>     >  1. `ssh-keygen -t rsa -C "p0ny233@github.com"`
>     >
>     >  2. 该命令执行后，会在`C:/Users/用户名/` 目录下生成 `.ssh` 文件夹
>     >
>     >  3. 然后将 `C:/Users/用户名/.ssh/` 目录下的 **所有文件清空** ，继续下面的步骤即可
>
>  1. 生成 Github 的钥匙
>     >  命令格式：`ssh-keygen -t rsa -f C:/Users/用户名/.ssh/id_rsa.github -C "p0ny233@github.com"`
>  2. 生成 Gitlab 的钥匙
>     >  命令格式：`ssh-keygen -t rsa -f C:/Users/用户名/.ssh/id_rsa.gitlab -C "fengyucong@gitlab.com"`
>  3. 生成 Gitee 的钥匙
>     >  命令格式：`ssh-keygen -t rsa -f C:/Users/用户名/.ssh/id_rsa.gitee -C "p0ny233@gitee.com"`

### 三、多账号必须配置 config 文件(重点)

>1. 创建 `config` 文件
>    查看 `"C:/Users/用户名/.ssh/`  目录下是否存在 `config` 文件，没有则创建 
>    
>    ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628681315136-image.png)
>    > `config` 文件的内容
>    >
>    > **里面的内容不能含有中文【未自测】**
>    >```
>		 > #Default gitHub user Self
>    > Host github.com
>	 	 >     HostName github.com
>		 >     User git #默认就是git，可以不写
>		 >     IdentityFile  C:/Users/admin/.ssh/id_rsa.github
>		 > 
>		 > #Add gitLab user 
>		 >     Host 10.11.10.37
>		 >     HostName gitlab.com
>		 >     User git
>		 >     IdentityFile  C:/Users/admin/.ssh/id_rsa.gitlab
>		 > 
>    > # gitee
>	 	 > Host gitee.com
>	 	 >     Port 22
>	   >     HostName gitee.com
>		 >     User git
>		 >     IdentityFile  C:/Users/admin/.ssh/id_rsa.gitee
>		 > 
>    >```
>		 >对 `config` 文件内容的字段说明：
>		 >  - Host
>		 >
>		 >    涵盖了下面一个段的配置，我们可以通过他来替代将要连接的服务器地址。
>		 >  这里可以使用任意字段或通配符。
>		 >  当ssh的时候如果服务器地址能匹配上这里Host指定的值，则Host下面指定的HostName将被作为最终的服务器地址使用
>        >
>		 >  - Port 
>		 >
>		 >    自定义的端口。默认为22，可不配置
>		 >  - User 
>		 >
>		 >    自定义的用户名，默认为git，可不配置
>		 >  - HostName 
>		 >
>		 >    真正连接的服务器地址
>		 >  - PreferredAuthentications 
>		 >
>		 >    指定优先使用哪种方式验证，支持密码和秘钥验证方式
>		 >  - IdentityFile 
>		 > 
>		 >    指定本次连接使用的密钥文件

### 四、在 github 、 gitlab 和 gitee 网站添加 ssh

>1. github直达地址：`https://github.com/settings/keys`
>   
>    ![github直达添加key地址](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628682745107-image.png)
>
>
>
>
>2. 将 `C:\Users\admin\.ssh\id_rsa.github.pub` 文件的所有内容复制粘贴到 文本框中
>
>    ![打开文件](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628683524975-image.png)
>
>    ![点击添加](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628683643213-image.png)
>

### 五、在 git 终端测试是否连接成功

>1. 由于每个托管商的**仓库**都有唯一的后缀，比如 Github 的是 git@github.com:*。
>所以可以这样测试：
> `ssh -T git@github.com`
>
>    ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628683925777-image.png)
>
>
>2. 然后遇到问题【如果没遇到问题则跳过】：`git@github.com: Permission denied (publickey).`
> 
>    ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628684125655-image.png)
>
>     解决方式：
>    > 1. 使用命令 `ssh-agent bash`  
>    > 2. 命令格式 `ssh-add C:/Users/用户名/.ssh/id_rsa.平台名称` 【**注意路径的斜杠**】
>    >    我这里的命令是 `ssh-add C:/Users/admin/.ssh/id_rsa.github`
>      ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628685369174-image.png)
>
>
> 3. 继续测试
> ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628685567778-image.png)
> 提示 `Hi 用户名! You've successfully authenticated, but GitHub does not provide shell access. ` 说明连接成功


### 六、测试内网 gitlab 连接

> 1. 无法使用命令 `ssh -T 10.11.10.37` 进行测试，会遭到**权限的拒绝**
> 2. 而且 `config` 配置文件中的 Host 字段需要指定成 **IP地址** ，否则也会遭到**权限的拒绝**
> 3. 只能添加 仓库的地址去 `pull` 下来，才知道是否成功   
>   3.1 复制仓库地址
>       ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628689656125-image.png)
>   3.2 新建一个空文件夹 ，然后打开 git终端使用初始化命令   
>       `git init`   
>   3.3 使用命令添加刚复制的远程仓库    
>`git remote add apk git@gitlab.testplus.cn:chenyang6/apk_checker.git`  
>   3.4 下载仓库中的代码   
>`git pull apk master` 





![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-11/1628689249564-image.png)































  