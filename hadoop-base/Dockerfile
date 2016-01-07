# Creates a base centos image with jdk and hadoop
FROM centos:7

MAINTAINER wziyong <wziyong@126.com>

#install ssh
RUN yum install -y openssh-clients

#install sshd 
RUN yum install -y openssh-server


#move all files into container /usr/local, including jdk ,configuration files and  hadoop pkg 
ADD files/* /usr/local/

# set jave environment variable 
ENV JAVA_HOME /usr/local/jdk1.7.0_80 
ENV PATH $PATH:$JAVA_HOME/bin
