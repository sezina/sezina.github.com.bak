---
layout: post
title: "Using Maven(2): pom.xml"
date: 2012-12-16 16:24
comments: true
categories: [Java, Maven, Eclipse]
---
## Coordinate(坐标)

```xml pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.foo.bar</groupId>
  <artifactId>mvnTest</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>mvnTest</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

这是Eclipse下创建Maven项目时自动产生的pom文件。Maven通过一组元素来定义当前项目的坐标，它们是：groupId, artifactId, version, packaging, classifier，简单说就是将来该项目在类库中的标识。之前那个项目的坐标如下：

```xml
  <groupId>com.foo.bar</groupId>
  <artifactId>mvnTest</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>
```

各元素的意思：

+ groupId: 定义当前Maven项目所属的实际项目。
+ artifactId: 定义实际项目中的一个Maven项目(模块)，推荐是用实际项目名称作为artifactId的前缀。
+ version: 项目版本。
+ packaging: Maven项目的打包方式。
+ classifier: 帮助定义构建输出的一些附属构件。

## 依赖配置

由pom文件中的```<dependencies></dependencies>```负责，下面的每个```<dependency></dependency>```定义一个包依赖，大体的结构如下：

```xml
    <dependency>
      <groupId>...</groupId>
      <artifactId>...</artifactId>
      <version>...</version>
      <type>...</type>
      <scope>...</scope>
      <optional>...</optional>
      <exclusions>
        <exclusion>
        ...
        </exclusion>
        ...
      </exclusions>
    </dependency>
```

每个元素的意义如下：

+ groupId, artifactId, version: 这是依赖的基本坐标，是必须的。
+ type: 对应packaging，默认为jar。
+ scope: 依赖作用的范围。
+ optional: 标记依赖是否可用。
+ exclusions: 用于排除传递性依赖。
