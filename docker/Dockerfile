FROM openjdk:8-jre-slim

##jmeter版本
ARG JMETER_VERSION=5.6.3

RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak
COPY sources.list /etc/apt/sources.list

#安装必要软件
RUN apt-get clean && \
    apt-get update && \
    apt-get -qy install \
                wget \
                telnet \
                net-tools \
                vim

#防止vim安装失败
RUN apt-get update --fix-missing && apt-get install -y vim --fix-missing

#安装jmeter
RUN mkdir /jmeter \
	&& cd /jmeter/ \
#	国内镜像高速下载jmeter
	&& wget https://mirrors.aliyun.com/apache/jmeter/binaries/apache-jmeter-$JMETER_VERSION.tgz \
#	官方下载jmeter
#	&& wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-$JMETER_VERSION.tgz \
	&& tar -xzf apache-jmeter-$JMETER_VERSION.tgz \
	&& rm apache-jmeter-$JMETER_VERSION.tgz

#JMETER_HOME环境变量
ENV JMETER_HOME /jmeter/apache-jmeter-$JMETER_VERSION/
ENV PATH $JMETER_HOME/bin:$PATH

WORKDIR $JMETER_HOME

#脚本复制到当前路径
COPY ./config/user.properties bin/user.properties
COPY ./scripts/install_plugin_manager.sh .
COPY ./scripts/entrypoint.sh .

RUN chmod +x install_plugin_manager.sh entrypoint.sh
RUN ./install_plugin_manager.sh

EXPOSE 60000 1099 50000
ENTRYPOINT ["entrypoint.sh"]
