+++
title = "Spark多节点配置"
date =  2016-03-14T16:22:23+02:00
tags = ["bigdata","framework"]
description = "Spark - Multinodes - start from 0"
# 此处可以写摘要！
categories = ["tech"]
menu = ""
banner = "banners/spark-multi-nodes.jpg"
+++

## 楔子

这次完全拿到的是裸机，所以从零开始配置。其实集群和单节点差不多，见我前面的blog

## 本机配置

 * Centos 5.8
 * 4 cores 8G

## 节点布置 Masters&Slaves

    Master 119.254.168.33
    Slaves1 119.254.168.34
    Slaves2 119.254.168.36
    Slaves3 119.254.168.38

## 环境配置 Environment
### JAVA 环境

    见Apache Spark单节点安装和环境配置

### SCALA 环境

    见Apache Spark单节点安装和环境配置

### SSH 配置

背景：搭建Hadoop环境需要设置无密码登陆，所谓无密码登陆其实是指通过证书认证的方式登陆 ，使用一种被称为”公私钥”(RSA)认证的方式来进行ssh登录。 在linux系统中,ssh是远程登录的默认工具,因为该工具的协议使用了RSA/DSA的加密算法.该工具做linux系统的远程管理是非常安全的。

所谓ssh就是ssh免密码登录服务器，其中用到了RSA加密算法。其中的细节和原理我有时间再写。

### 确保安装好

    ssh：（ubuntu版）
    $ sudo apt-get update
    $ sudo apt-get install openssh-server
    $ sudo /etc/init.d/ssh start

    ssh(centos): 确认系统已经安装了SSH。
    rpm –qa | grep openssh
    rpm –qa | grep rsync


    yum install ssh //安装SSH协议
    yum install rsync //rsync是一个远程数据同步工具，可通过LAN/WAN快速同步多台主机间的文件
    service sshd restart –>启动服务 2. 生成并添加密钥：

    $ ssh-keygen -t rsa
    $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    $ chmod 0600 ~/.ssh/authorized_keys
    service sshd restart //一般修改过都需要重启服务

如果已经生成过密钥，只需执行后两行命令。
测试ssh localhost

    $ ssh localhost
    $ exit

查看端口：是否打开

    netstat -anp |grep ssh
    Hadoop cluster Installation

基本和前面相同

修改hdfs-site.xml

    <configuration>
        <property>
            <name>dfs.namenode.secondary.http-address</name>
            <value>kexinyun1:9001</value>
        </property>


        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:///opt/hadoop-2.6.1/dfs/name</value>
        </property>


        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:///opt/hadoop-2.6.1/dfs/data</value>
        </property>


        <property>
            <name>dfs.replication</name>
        <value>3</value>
        </property>


        <property>
            <name>dfs.webhdfs.enabled</name>
        <value>true</value>
        </property>

mapred-site.xml

    <configuration>  
        <property>  
    <name>mapreduce.framework.name</name>  
    <value>yarn</value>  
        </property>  
    <property>  
        <name>mapreduce.jobtracker.http.address</name>  
        <value>nameNode:50030</value>  
    </property>  
    <property>  
        <name>mapreduce.jobhistory.address</name>  
        <value>nameNode:10020</value>  
    </property>  
    <property>  
        <name>mapreduce.jobhistory.webapp.address</name>  
        <value>nameNode:19888</value>  
    </property>  
    </configuration> 

yarn 修改

    <configuration>
        <!-- Site specific YARN configuration properties -->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>kexinyun1:8032</value>
        </property>
        <property>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>kexinyun1:8030</value>
        </property>
        <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>kexinyun1:8031</value>
        </property>
        <property>
            <name>yarn.resourcemanager.admin.address</name>
        <value>kexinyun1:8033</value>
        </property>
        <property>
            <name>yarn.resourcemanager.webapp.address</name>
            <value>kexinyun1:8088</value>
        </property>
    </configuration>

slaves 文件

    119.254.168.38 //(slaves3)
    119.254.168.36 //(slaves2)
    119.254.168.34 //(slaves1)

    vi hadoop-env.sh
    export JAVA_HOME=your java home
    vi yarn-env.sh

    export JAVA_HOME=your java home   

格式化（同以前）
启动/停止

查看：jsp

访问:

http://ip:9001/

Spark Cluster Installation

基本同单节点类似
文件配置部分

    export SCALA_HOME=/opt/scala-2.11.4
    export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.101.x86_64/
    export SPARK_HOME=/opt/spark
    export YARN_CONF_DIR=$HADOOP_HOME/etc/hadoop
    export SPARK_JAR=/opt/spark/lib/spark-assembly-1.6.1-hadoop2.6.0.jar
    export SPARK_MASTER_IP=localhost
    export SPARK_MASTER_PORT=7077
    export SPARK_WORKER_CORES=1
    export SPARK_WORKER_INSTANCES=1
    export SPARK_WORKER_MEMORY=1g

启动

    $SPARK_HOME/sbin/start-all.sh

## problem

http://www.2cto.com/os/201209/155681.html

english version

http://pingax.com/install-hadoop2-6-0-on-ubuntu/

## Reference

http://www.cnblogs.com/lanxuezaipiao/p/3525554.html
http://pingax.com/install-hadoop2-6-0-on-ubuntu/
http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/ClusterSetup.html
http://blog.csdn.net/greensurfer/article/details/39450369


