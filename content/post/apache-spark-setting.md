+++
title =  "Apache Spark安装和环境配置"
date =  2016-01-08T13:26:32+02:00
tags = ["framework","bigdata"]
categories = ["tech"]
description = ""
# 此处可以写摘要！
menu = ""
banner = "banners/apache-setting.jpg"
+++

# Excerpt

**The Desiderata (part 2)**

   *by Max Ehrmann, 1927*

> If you compare yourself with others,

> you may become vain or bitter;

> for always there will be greater and lesser persons than yourself.

> Enjoy your achievements as well as your plans.

> Keep interested in your own career, however humble;

> it is a real possession in the changing fortunes of time.  –TBC

# 安装方式

* 源码编译

需要从github上clone下来， 通过maven 或者 sbt 进行编译。读者可以到官网 上进行下载。

如果需要自己编译，那么需要配置环境（见下）， 然后通过 sbt 进行编译（见下）。当然如果觉得太麻烦可以直接下载预编译的版本。

* 预编译的版本

Prebuilt 版本就方便多了spark-1.6.0-bin-hadoop2.6.tgz

1. 下载到制定位置

2. 解压

3. 运行shell

> cd spark/bin

>./spark-shell

在shell脚本中可以看到很多信息，openjdk版本， scala版本等等。 同时可以通过web 的UI界面访问：http://localhost:4040/jobs/

# 环境构建

1. spark 支持多个版本的Hadoop， 无论是 Apache Hadoop 还是Cloudera 的CDH， Hartonworks的。 你用什么版本的hadoop所以问题不大

2. Spark runs on both Windows and UNIX-like systems (e.g. Linux, Mac OS). 所以window 就不行了，支持Unix家族的机器 Mac， Linux（Ubuntu， Redhat， Centos等）.

3. Spark runs on Java 7+, Python 2.6+ and R 3.1+. Spark 1.6.0 uses Scala 2.10. 所以Scala 配置 2.10.×版本的。

# 编译工具安装 - sbt

spark 官网上推荐 maven或者sbt 进行编译scala文件。 我个人推荐用sbt，轻便简单。但是容易在安装时遇到问题（gfw）。 sbt 在之后 运行spark工程时也需要，编译打包放在spark submit 上运行。

官网下载地址
http://www.scala-sbt.org/

运行安装包


# 创建工程：

1. 新建工程文件夹

比如现在的工程名为“sparksample”。那么

    cd sparksample
    mkdir project
    mkdir src/main/scala

一般的工程文件结构如下：

* project – 工程定义文件

* project/build/.scala – 主要的工程定义文件

* project/build.properties – 工程，sbt以及scala版本定义

* src/main – 你的应用代码放在这里，不同的子目录名称表示不同的编程语言（例如，src/main/scala,src/main/java)

* src/main/resources – 你想添加到jar包里的静态文件（例如日志配置文件）

* lib_managed – 你的工程所依赖的jar文件。会在sbt更新的时候添加到该目录

* target – 最终生成的文件存放的目录（例如，生成的thrift代码，class文件，jar文件）

2. 编写build.sbt文件

        name := "SparkSample"
        version := "1.0"
        scalaVersion := "2.10.3"
        libraryDependencies += "org.apache.spark" %% "spark-core" % "1.1.1"

这里需要注意使用的版本，scala 和spark streaming的版本是否匹配等等。

查看地址： http://mvnrepository.com/artifact/org.apache.spark/spark-streaming_2.10/1.4.1

3. 构建jar 包

在project的文件目录下（e.g. “sparksample”）

        sbt package
提交到spark submit：

        cd /opt/spark-verisonnuber/bin/
        ./spark-submit --class "org.apache.spark.examples.streaming.sparksample" --packages org.apache.spark:spark-streaming-kafka_2.10:1.4.1 --master local[2]  /home/sherry/sparksample/target/scala-2.10/sparksample-1.0.jar 10.81.52.88:9092 tintin  

具体怎么写参数，请看官方：

http://spark.apache.org/docs/latest/submitting-applications.html#submitting-applications

**注意**： 略坑的是， 需要将调用的包手动加入 – packages 


# reference

1. [我的csdn博客](http://blog.csdn.net/u011613321)

2. http://www.tuicool.com/articles/AJnIvq

3. http://www.scala-sbt.org/release/docs/index.html

4. http://www.supergloo.com/fieldnotes/apache-spark-cluster-part-2-deploy-a-scala-program-to-spark-cluster/