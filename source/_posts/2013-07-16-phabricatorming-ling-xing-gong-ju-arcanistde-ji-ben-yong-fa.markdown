---
layout: post
title: "Phabricator命令行工具Arcanist的基本用法"
date: 2013-07-16 20:49
comments: true
categories: [Phabricator, Code review]
---

[Pharicator][phabricator]是FB的代码审查工具，现在我所在的团队也使用它来进行代码质量的控制。其提供了一个differential(code review)命令行工具Arcanist(arc)。本文仅从本人的日常使用中总结出Arcanist比较常用的用法做个简单介绍。

**环境说明**

+ OS: OS X Mountail Lion
+ SCV: svn
+ IDE: Eclipse

## 安装

+ 将Arcanist的源码拷贝到本地

```bash
somewhere/ $ git clone git://github.com/facebook/libphutil.git
somewhere/ $ git clone git://github.com/facebook/arcanist.git
```

+ 将arc的路径加入到系统路径中

```bash
$ export PATH=$PATH:/somewhere/arcanist/bin/
```

或在系统的profile或是bash(如果用bash)的配置文件的末尾加上这一句。

+ 命令行中输入`arc`看提示确认是否安装成功。

## arc配置

+ arc的全局配置

配置arc的默认编辑器，我使用vim
```bash
$ arc set-config editor "vim"
```

配置默认的phabricator的uri，uri为团队的phabricator主页的url
```bash
$ arc set-config default http://phabricator.example.com
```

+ 在项目的根目录下建`.arcconfig`配置文件，文件中至少要填入以下内容

```json .arcconfig
{
	"project_id" : "your project name",
	"conduit_uri" : "your phabricator url"
}
```

举个例子：

```json .arcconfig
{
	"project_id" : "HelloWorld",
	"conduit_uri" : "http://phabricator.example.com"
}
```

该配置文件还可以配置静态代码检测引擎(lint)和单元测试引擎。

+ 为项目安装证书，用于phabricator的认证。

```bash
yourproject/ $ arc install-certificate
```

接着按照命令行提示操作就OK了。

弄完这一步，才能真正在项目中使用arc。

## 在项目中使用arc

+ `arc help [--full | [COMMAND]]` 查看帮助文档，接参数`--full`查看所有命令的详细用法，接具体的命令`[COMMAND]`如`arc help diff`可以查看该命令的详细用法。
+ 想phabricator提交review request(Differential).修改完代码后，使用`arc diff <path>`命令提交review request，该命令会产生一个包含如下内容的文件要求填写：
```
<<Enter Revision Title>>         
 
Summary:
 
Test Plan:
 
Reviewers:
 
CC:

Maniphest Tasks:
 
# NEW DIFFERENTIAL REVISION
# Describe the changes in this new revision.
#
# arc could not identify any existing revision in your working copy.
# If you intended to update an existing revision, use:
#
#   $ arc diff --update <revision>
```
按照提示填写后，保存退出，arc就会自动提交request。`Reviewers`用逗号隔开，`Maniphest Tasks`填相关联的phabricator上的`task_id`，如`T100`。Test plan暂时没用过，官方文档：[http://www.phabricator.com/docs/phabricator/article/Differential_User_Guide_Test_Plans.html][testplan]

提交完成后，会产生一个形如`http://phabricator.example.com/D24`的url，url中的`D24`是revision_id。

+ `arc diff --update <revision_id>`更新<revision_id>对应的review request。该命令产生一个如下的文件，按提示填写保存退出，arc会提交更新。
```


# Updating D27: hahahah
#
# Enter a brief description of the changes included in this update.
# The first line is used as subject, next lines as comment.
#
# If you intended to create a new revision, use:
#  $ arc diff --create
```

+ `arc commit --revision <revision_id>`提交对应<revision_id>提交代码更改，这个命令把svn commit的工作也做掉了，直接提交到代码库。
+ `arc todo <description> [option]`可以快速给自己在phabricator上创建task，`[option]`用于把task CC给其他人.
+ `arc tasks [options]` 查看Maniphest的tasks。
+ `arc amend --show` 查看当前项目的differentials，`arc amend --revision <revision_id> --show` 查看指定revision_id的differential。


**Reference:**

1. Arcanist官方文档： [http://www.phabricator.com/docs/phabricator/article/Arcanist_User_Guide.html](http://www.phabricator.com/docs/phabricator/article/Arcanist_User_Guide.html) 


[phabricator]: http://phabricator.org "Phabricator"
[testplan]: http://www.phabricator.com/docs/phabricator/article/Differential_User_Guide_Test_Plans.html
