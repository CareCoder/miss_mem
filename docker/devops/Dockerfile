FROM 172.26.143.190:10020/demeter/server/jre8
MAINTAINER server

ADD ./target/ROOT.jar /runapp/
ADD ./ext-libs/ /skywalking/ext-libs/
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' >/etc/timezone \
EXPOSE 8080
# RUN chmod +x /bin/sh
# Define default command
# ENTRYPOINT java -jar -Dlogging.config=classpath:logback.xml /runapp/ROOT.jar
ENTRYPOINT exec java -javaagent:/skywalking/ext-libs/skywalking-agent.jar -Dskywalking.agent.service_name=demeter-server -Dskywalking.collector.backend_service=172.26.143.191:11800 -jar /runapp/ROOT.jar