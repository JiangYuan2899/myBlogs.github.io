---
layout: post
title: Maven命令
category: other
tags: [other]
---
常用命令

( 1 ) mvn clean 删除工程的target 目录下的所有文件。

( 2) mvn package 将工程打为Jar 包。

    mvn package 命令执行过程包含以下6 个阶段。

验证。

编译代码。

处理代码。

生成资源文件。

生成Jar 包。

测试。

( 3 ) mvn package -Dmaven . test.skip=ture ，打包时跳过测试。

( 4 ) mvn compile 编译工程代码， 不生成Jar 包。

( 5 ) mvn install 命令包含了mvn package 的所有过程， 并且将生成的Jar 包安装到本地仓

库。执行mvn install 命令， 可以看到终端输出的日志， 经过了与mvn package 相同的阶段， 最

后将Jar 包安装到本地仓库

( 6 ) mvn spring-boot:run 使用spring-boot 插件， 启动Spring Boot 工程。该命令执行时先

检查Spring Boot 工程源码是否编译，如果工程源码没有编译，则先编译；如果编译了；则启

动工程。

。

( 7) mvn test 测试。

( 8) mvn idea: idea 生成idea 项目。

( 9) mvnjar:jar 只打Jar 包。

( 10) mvn validate 检验资源是否可用。