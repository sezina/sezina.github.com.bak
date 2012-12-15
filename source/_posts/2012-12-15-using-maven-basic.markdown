---
layout: post
title: "Using Maven(1): Basic"
date: 2012-12-15 12:18
comments: true
categories: [Java, Eclipse, Maven]
---

Maven是Apache的一个开源项目，主要用于Java项目的构建、依赖管理和项目信息管理。Maven是个异常强大的自动化构建工具。这篇博文是我在开发过程中使用Maven的学习笔记。

环境：

+ ubuntu 12.04
+ Eclipse J2EE版本(惭愧只用过Eclipse)
+ Maven 3.0.0+

## Install
需要安装的有两个东西：一个是Maven本身，一个是IDE插件。

1. 从Apache的[Maven官网][maven]下载压缩包，并解压到指定目录.
2. 将Maven添加到系统路径中：export PATH=/path/to/maven/bin:$PATH 。如果使用bash，将该条语句添加到.bashrc中；zsh的添加到.zshrc中。搞定这步就可以在命令行使用Maven了。
3. 命令行虽然方便快捷，但使用Java进行开发的一般还是会用IDE进行开发。有些版本的Eclipse自带了maven插件，不好用，还是找个没有自带的自己装吧。m2e：http://m2eclipse.sonatype.org/sites/m2e，这个是必需的；m2e-extra：http://m2eclipse.sonatype.org/sites/m2e-extras，这个不是必需的。装完后，重启Eclipse，右键点击项目会发现多了Maven选项。
4. Eclipse里还用进行一下设置，将maven的配置文件换成我们下的maven的配置文件。配置：Maven > installations > Add…。选到maven的目录添加进来就行了。

## Usage
Maven也是遵循“约定由于配置”原则的，所以使用Maven管理项目时，有约定好的目录结构。说实话，有关java的东西的命令行我都不熟，这里只说说Maven常用的几个命令和Eclipse下的使用。Eclipse下使用Maven创建的Java项目目录结构：
    
    /src/main/java        ＃项目主代码目录
    /src/test/java        ＃项目测试代码目录
    /target               ＃maven的输出目录
    pom.xml               ＃maven中最重要的文件，作用如build.xml之于Ant

一般我们还会创建一个/src/main/resources目录用于存放项目资源，这也是maven约定的资源存放目录。

### Maven常用命令
1. mvn help:system  该命令会答应出系统的环境变量，有时会非常有用。
2. mvn clean compile 该命令会清理输出目录/target并编译项目至/target/classes目录。
3. mvn clean test 清理输出目录->处理主资源->编译主代码->处理测试资源->编译测试代码->测试。
4. mvn clean package 打包命令，默认打包成jar包。打包后的包名为pom.xml文件定义的artifactId-version.jar。
5. mvn clean install 安装命令，会将当前项目打包后安装到Maven的本地仓库中，使得可以在其他maven项目中使用该包。

Maven的命令还有其他参数可选，根据项目需求查看文档选择所需参数。

### 在Eclipse中使用Maven
1. 使用maven创建项目：
{% img /images/mvn-project-select.png %}
2. 选择archetype来生成项目骨架：
{% img /images/mvn-artifact-select.png %}
3. 填写几个关键信息：
{% img /images/mvn-project-info.png %}
4. 点击finish，这样就创建了一个Maven项目。Eclipse已经集成了Maven的几个常用命令，只需使用右键菜单选择即可执行。



[maven]: http://maven.apache.org “maven”
