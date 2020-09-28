FROM centos:centos7.7.1908
MAINTAINER server

# add java and tomcat support in the container
ADD ./install/jdk-8u241-linux-x64.tar.gz  /usr/java
#ADD ./install/locale /etc/default/

# configuration of java and tomcat ENV
ENV LANG en_US.UTF-8
ENV JAVA_HOME /usr/java/jdk1.8.0_241
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:$JAVA_HOME/bin
