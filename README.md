# automaticDeploy
大数据环境一键安装脚本

master分支的hive安装脚本在安装hive时只能适配1.x，而hive分支的脚本可以适配2.x以及较新版本的安装。

推荐使用**hive分支**的脚本，增加了更多组件的支持，master分支的代码为了保持和教程的同步，暂时不会做大的更新。

# 适用环境
CentOS 7以上

# 使用方法
1. 在/home下创建hadoop目录，用于存放脚本

```
mkdir /home/hadoop
```

2. 下载脚本到/home/hadoop目录下

```
cd /home/hadoop
git clone https://github.com/MTlpc/automaticDeploy.git
# 如果未安装 git
yum install git -y
```

3. 进入到/home/hadoop/automaticDeploy目录下，配置host_ip.txt

```
# 配置集群信息，格式为：ip hostname user password
192.168.31.41 node01 root 123456
192.168.31.42 node02 root 123456
192.168.31.43 node03 root 123456
```

4. 将对应组件的安装包放置到 `/home/hadoop/automaticDeploy/frames` 目录中
> 虚拟机可以本地上传，服务器的话可以使用 wget 下载

5. 配置 `frames.txt`，填写安装包全称，以及需要安装的节点

```
# 通用环境
jdk-8u144-linux-x64.tar.gz true
azkaban-sql-script-2.5.0.tar.gz true
# Node01
hadoop-2.7.7.tar.gz true node01
# Node02
mysql-rpm-pack-5.7.28 true node02
azkaban-executor-server-2.5.0.tar.gz true node02
azkaban-web-server-2.5.0.tar.gz true node02
presto-server-0.196.tar.gz true node02
# Node03
apache-hive-1.2.1-bin.tar.gz true node03
apache-tez-0.9.1-bin.tar.gz true node03
sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz true node03
yanagishima-18.0.zip true node03
# Muti
apache-flume-1.7.0-bin.tar.gz true node01,node02,node03
zookeeper-3.4.10.tar.gz true node01,node02,node03
kafka_2.11-0.11.0.2.tgz true node01,node02,node03
```

6. 如安装 mysql、azkaban，需配置 `configs.txt`，填写相关配置

```
# Mysql相关配置
mysql-root-password DBa2020*
mysql-hive-password DBa2020*
mysql-drive mysql-connector-java-5.1.26-bin.jar
# azkaban相关配置
azkaban-mysql-user root
azkaban-mysql-password DBa2020*
azkaban-keystore-password 123456
```

7. 进入 `systems` 目录执行 `batchOperate.sh` 脚本初始化环境

```
cd systems
chmod -x *
./batchOperate.sh
```

8. 进入 `hadoop` 目录中，选择对应组件的安装脚本，依次进行安装（需要在各个节点执行）

```
cd hadoop
chmod -x *
# 安装flume
./installFlume.sh
# 安装zookeeper
./installZookeeper.sh
# 安装kafka
./installKafka.sh
```

# 致谢

项目基于 [BigData_AutomaticDeploy](https://github.com/SwordfallYeung/BigData_AutomaticDeploy) 开发而成，当我有了写一键搭建脚本的时候，在github上搜索到的一个项目，帮我减少了很多造轮子的时间，非常感谢。

在此基础上，增加了不少的大数据组件，并适配了CentOS7 1511，并做了不少的改动。

这个项目，同样会持续开源和维护，后续会增加更多的大数据组件。再次感谢！！！