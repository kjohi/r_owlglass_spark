FROM apache/zeppelin:0.7.3
MAINTAINER ko <kolin.ohi@gmail.com>
ENV ZEPPELIN_PORT="8889"

RUN mkdir -p /root/.aws
ADD resources/interpreter.json /zeppelin/conf/
ADD resources/zeppelin-site.xml /zeppelin/conf/
ADD resources/requirements.txt /root/
ADD resources/credentials /root/.aws
ADD resources/config /root/.aws

RUN pip install -r /root/requirements.txt


ENV SPARK_VERSION 2.3.1
ENV HADOOP_PROFILE 2.7
ENV HADOOP_VERSION 2.7.3
ENV HADOOP_PREFIX /usr/local/hadoop
ENV HADOOP_COMMON_HOME /usr/local/hadoop
ENV HADOOP_HDFS_HOME /usr/local/hadoop
ENV HADOOP_MAPRED_HOME /usr/local/hadoop
ENV HADOOP_YARN_HOME /usr/local/hadoop
ENV HADOOP_CONF_DIR /usr/local/hadoop/etc/hadoop
ENV HADOOP_HOME=/usr/local/hadoop 
ENV SPARK_HOME /usr/local/spark
ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop


# Update the image with the latest packages
RUN apt-get update && apt-get install -y wget tar curl gzip libssl-dev 

# install hadoop 2.7.3
RUN curl -s https://archive.apache.org/dist/hadoop/core/hadoop-$HADOOP_VERSION/hadoop-$HADOOP_VERSION.tar.gz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s ./hadoop-$HADOOP_VERSION hadoop

ADD resources/hdfs_conf/core-site.xml $HADOOP_PREFIX/etc/hadoop/core-site.xml
ADD resources/hdfs_conf/hdfs-site.xml $HADOOP_PREFIX/etc/hadoop/hdfs-site.xml
ADD resources/hdfs_conf/mapred-site.xml $HADOOP_PREFIX/etc/hadoop/mapred-site.xml
ADD resources/hdfs_conf/yarn-site.xml $HADOOP_PREFIX/etc/hadoop/yarn-site.xml

#install spark

RUN cd /tmp && \
        wget -q http://apache.mirrors.pair.com/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop${HADOOP_PROFILE}.tgz && \
        echo "DC3A97F3D99791D363E4F70A622B84D6E313BD852F6FDBC777D31EAB44CBC112CEEAA20F7BF835492FB654F48AE57E9969F93D3B0E6EC92076D1C5E1B40B4696 *spark-${SPARK_VERSION}-bin-hadoop${HADOOP_PROFILE}.tgz" | sha512sum -c - && \
        tar xzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_PROFILE}.tgz -C /usr/local && \
        rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_PROFILE}.tgz
RUN cd /usr/local && ln -s spark-${SPARK_VERSION}-bin-hadoop${HADOOP_PROFILE} spark


# Spark config
ENV SPARK_HOME /usr/local/spark
ENV PYTHONPATH $SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.4-src.zip
ENV SPARK_OPTS --driver-java-options=-Xms1024M --driver-java-options=-Xmx4096M --driver-java-options=-Dlog4j.logLevel=info

# RUN cp -r /zeppelin/tempy/test/notebook/* /zeppelin/notebook/

EXPOSE 8889

ENTRYPOINT [ "/usr/bin/tini", "--" ]
WORKDIR ${Z_HOME}
CMD ["bin/zeppelin.sh"]
