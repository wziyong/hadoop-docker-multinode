# Hadoop Docker 跨主机集群安装及benchmark测试
搭建跨主机、Docker上部署Hadoop集群，并进行benchmark测试，以研究Hadoop+Docker的性能到底如何。

##项目背景
最近有意研究一下Hadoop，在了解了hadoop原理之后，决定把hadoop集群搭建起来。起初，在物理机上创建了3台虚拟机，并部署了3个节点的Hadoop集群，简单跑了一下Demo，对Hadoop的机制有了更进一步理解。我想，既然Hadoop是为大数据而生，3个节点并不能感受Hadoop的强大，有些问题可能只会在集群规模上来的时候才会出现。于是，我决定增大集群规模，目标是达到100个节点。考虑到虚拟机比较笨重，以现有的资源是不能搭建出100个节点的集群。Docker作为容器虚拟化技术，有轻量、占中资源少、部署方便、方便共享等特点，我决定使用Hadoop+Docker的方式来搭建大规模的集群。参考了网上众多的资料，终于搭建成功了，在此记录一下，希望能给大家一点帮助。

-----------------

目录
-----------------
- [实验条件&&系统结构](part1-what-is-a-log.md)
- [第一部分：安装准备](part1-what-is-a-log.md)
    1. [Docker安装](part1-what-is-a-log.md#数据库中的日志)
    1. [hadoop-docker-multinode下载](part1-what-is-a-log.md#变更日志changelog101表与事件的二象性duality)
    2. [相关安装包下载](part1-what-is-a-log.md#变更日志changelog101表与事件的二象性duality)
    1. [接下来的内容](part1-what-is-a-log.md#接下来的内容)
- [第二部分：集群安装](part2-data-integration.md)
    1. [DNS搭建](part2-data-integration.md#数据集成两个难题)
    1. [Docker网络设置](part2-data-integration.md#日志结构化的log-structured数据流)
    1. [hadoop-base镜像](part2-data-integration.md#在linkedin)
    1. [hadoop-master镜像](part2-data-integration.md#etl与数据仓库的关系)
    1. [hadoop-salve镜像](part2-data-integration.md#日志文件与事件)
    1. [搭建hadoop集群](part2-data-integration.md#构建可伸缩的日志)
    2. [接下来的内容](part1-what-is-a-log.md#接下来的内容)
- [第三部分：benchmark测试](part3-logs-and-real-time-stream-processing.md)
    1. [TeraSort](part3-logs-and-real-time-stream-processing.md#数据流图data-flow-graphs)
- [结束语及参考资料](the-end.md)
    1. [学术论文、系统、讨论和博客](the-end.md#学术论文系统讨论和博客)
    1. [一些相关的开源软件](the-end.md#一些相关的开源软件)


