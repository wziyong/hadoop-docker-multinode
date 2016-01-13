# Hadoop Docker 跨主机集群安装及benchmark测试
实现跨主机、Docker上部署Hadoop集群

##写在前面
    最近有意研究一下Hadoop，在了解了hadoop原理之后，决定把hadoop集群搭建起来。起初，在物理机上创建了3台虚拟机，并部署了3个node的Hadoop集群，简单跑了一下benchmark，对Hadoop的机制有了部分理解。
    我想，既然Hadoop是为大数据而生，3个节点并不能感受Hadoop的强大。为了更深入的理解Hadoop，我决定增大集群规模，目标是达到100个node。
