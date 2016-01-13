# Hadoop Docker 跨主机集群安装及benchmark测试
搭建跨主机、Docker上部署Hadoop集群，并进行benchmark测试，以研究Hadoop+Docker的性能到底如何。

##项目背景
最近有意研究一下Hadoop，在了解了hadoop原理之后，决定把hadoop集群搭建起来。起初，在物理机上创建了3台虚拟机，并部署了3个节点的Hadoop集群，简单跑了一下Demo，对Hadoop的机制有了更进一步理解。我想，既然Hadoop是为大数据而生，3个节点并不能感受Hadoop的强大，有些问题可能只会在集群规模上来的时候才会出现。于是，我决定增大集群规模，目标是达到100个节点。考虑到虚拟机比较笨重，以现有的资源是不能搭建出100个节点的集群。Docker作为容器虚拟化技术，有轻量、占中资源少、部署方便、方便共享等特点，我决定使用Hadoop+Docker的方式来搭建大规模的集群。参考了网上众多的资料，终于搭建成功了，在此记录一下，希望能给大家一点帮助。

我尝试了使用sequenceiq/ambari进行集群的部署，下载镜像的速度很慢，最终下载完成了，运行也是失败了。最终只能放弃了。。。
网上的资料很多，最终参考了下面两个项目完成了集群的搭建，但是有问题，不太好用：

1. [alvinhenrick/hadoop-mutinode](https://github.com/alvinhenrick/hadoop-mutinode)
1. [kiwenlau/hadoop-cluster-docker](https://github.com/kiwenlau/hadoop-cluster-docker)

[kiwenlau/hadoop-cluster-docker大体原理](http://kiwenlau.com/2015/06/08/150608-hadoop-cluster-docker/)
```
容器启动时，serf服务会立即启动，master节点的IP会传给所有slave节点。
slave节点上的serf agent会马上发现master节点，master节点就马上发现了所有slave节点。
然后它们之间通过互相交换信息，所有节点就能知道其他所有节点的存在了。
serf发现新的节点时，就会重新配置dnsmasq,然后重启dnsmasq.
这个过程随着节点的增加会耗时更久，稍等片刻才能启动Hadoop。
这个解决方案是由SequenceIQ公司提出的，该公司专注于将Hadoop运行在Docker中。
```

就像kiwenlau所说，随着集群节点的增加，serf发现并同步时间很长，比如，我在一台节点上安装30个节点时，通过serf members命令查看节点状态，一直是不稳定的，有时是active，有时是inactive状态。单主机上部署30个节点，由于网络的原因，系统是不可用的。而且跨主机也没有涉及。所以，我在他们基础上做了一些修改，比如舍弃serf，搭建单独dns服务器，实现多主机docker容器互联，更换了镜像系统为centos7（因为自己习惯centos了）等。后面将继续在上面进行hadoop的一些基准测试，以对hadoop+docker有个直观的认识。


## 欢迎参与
   - 如果您有一些建议、问题或者需要帮助
   - 如果您发现问题、bug
   - Request a New Feature
   
都可以创建一个issue，共同探讨。谢谢！


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
- [后面的工作]
    1. [Spark+Docker](xxx)
- [结束语及参考资料](the-end.md)
    1. [学术论文、系统、讨论和博客](the-end.md#学术论文系统讨论和博客)
    1. [一些相关的开源软件](the-end.md#一些相关的开源软件)


