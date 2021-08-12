---
title: Android Studio 配置问题 > The specified Gradle distribution
date: 2021-08-12 20:08:19
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
categories: AS
# 文章摘要 #
description: The specified Gradle distribution
# 文章置顶 #
sticky: 1
---

# Android Studio 配置问题 > The specified Gradle distribution
#### 问题如下
>一般这种情况是由于 **拉取别人的项目** 然后 **在自己的电脑中导入** 发生的
>
> ```
> The specified Gradle distribution 'file:///E:/DevelopKitTools/Android_Studio/gradle/wrapper/dists/gradle-6.5-all/2oz4ud9k3tuxjg84bbf55q0tn/gradle-5.4-all.zip' does not exist.
> ```
> 导入项目之前的配置文件内容如下：
>    ![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628759295229-image.png)
>
>
> **注意其中的classpath字段的值：`com.android.tools.build:gradle:3.4.0`**，表明：**Android Gradle Plugin Version 的版本号：3.4.0**   
>
>会自动下载，但是这里需要断掉它，因为本机没有 `gradle-5.1.1-bin.zip` 文件,这里的思路是把 配置文件 修改成 本机上有的`gradle-5.4.1-all.zip` 

>所有这里只需要分别修改两个文件
> 1. `F:\TempCode\项目名\build.gradle`  
    > 我这里的路径是：`F:\TempCode\MyApplicationqqLogin\build.gradle`
> 2. `F:\TempCode\项目名\gradle\wrapper\gradle-wrapper.properties`  
    > 我这里的路径是：`F:\TempCode\MyApplicationqqLogin\gradle\wrapper\gradle-wrapper.properties`
    
>
>
> gradle 对应版本`https://developer.android.com/studio/releases/gradle-plugin?hl=zh-cn`   
>
> 从上面的网址可以知道 `gradle-5.4.1-all.zip` 对应的 gradle插件版本：`3.5.0 - 3.5.4`   
>
>`build.gradle` 文件内容如下：
>```gradle
>// Top-level build file where you can add configuration options common to all sub-projects/modules.
>
>buildscript {
>    repositories {
>        google()
>        jcenter()
>        
>    }
>    dependencies {
>        classpath 'com.android.tools.build:gradle:3.4.0'
>        
>        // NOTE: Do not place your application dependencies here; they belong
>        // in the individual module build.gradle files
>    }
>}
>
>allprojects {
>    repositories {
>        google()
>        jcenter()
>        
>    }
>}
>
>task clean(type: Delete) {
>    delete rootProject.buildDir
>}
>
>```
### 修改 build.gradle 文件后的内容如下：
>```gradle
>// Top-level build file where you can add configuration options common to all sub-projects/modules.
>
>buildscript {
>    repositories {
>        google()
>        jcenter()
>        
>    }
>    dependencies {
>        classpath 'com.android.tools.build:gradle:3.5.4'
>        
>        // NOTE: Do not place your application dependencies here; they belong
>        // in the individual module build.gradle files
>    }
>}
>
>allprojects {
>    repositories {
>        google()
>        jcenter()
>        
>    }
>}
>
>task clean(type: Delete) {
>    delete rootProject.buildDir
>}
>
>```
>


>`gradle-wrapper.properties` 文件内容如下：
>```yaml
>#Sun Jun 20 14:54:20 CST 2021
>distributionBase=GRADLE_USER_HOME
>distributionPath=wrapper/dists
>zipStoreBase=GRADLE_USER_HOME
>zipStorePath=wrapper/dists
>distributionUrl=https\://services.gradle.org/distributions/gradle-5.1.1-all.zip
>```
>
### 修改 gradle-wrapper.properties 文件后的内容如下：
>`gradle-wrapper.properties` 文件内容如下：
>```yaml
>#Sun Jun 20 14:54:20 CST 2021
>distributionBase=GRADLE_USER_HOME
>distributionPath=wrapper/dists
>zipStoreBase=GRADLE_USER_HOME
>zipStorePath=wrapper/dists
>distributionUrl=file:///E:/DevelopKitTools/Android_Studio/gradle/wrapper/dists/gradle-5.4.1-all/3221gyojl5jsh0helicew7rwx/gradle-5.4.1-all.zip
>```



![gradle插件对应的版本](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628760070992-image.png)


![The specified Gradle distribution](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628757287484-image.png)

### 删除 `.gradle` 这个文件 【重点】
1. 右键 `.gradle` 文件夹，然后  点击删除
![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628769357516-image.png)

2. 点击 `file` > `Invalidate Claches / Restart`

![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628769498403-image.png)


3. 最后 点击 弹框的 `Invalidate and Restart`


![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628769519191-image.png)

4. 等待。。。就好了

注意整个项目的路径是需要放到 **没有中文的路径的**否则报错如图：

![](https://gitee.com/coder_p0ny/md-nice-markdown_pic/raw/master/2021-8-12/1628769770869-image.png)

解决方式就是：把项目安排到没有中文路径的即可



    



