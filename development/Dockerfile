FROM jupyter/base-notebook:ubuntu-20.04

USER root
RUN mkdir /opt/spark /opt/hadoop

RUN wget https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz --no-check-certificate  --directory-prefix=/tmp/ && tar -xvzf /tmp/spark-3.2.0-bin-hadoop3.2.tgz -C /opt/spark --strip-components 1  && rm /tmp/*.tgz

RUN wget https://archive.apache.org/dist/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz  --no-check-certificate  --directory-prefix=/tmp/ &&  tar -xvzf /tmp/hadoop-3.2.0.tar.gz -C /opt/hadoop --strip-components 1 && rm  /tmp/*.gz


RUN wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.11.0/jmx_prometheus_javaagent-0.11.0.jar  --no-check-certificate  --directory-prefix=/opt/spark/jars/

RUN wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar  --no-check-certificate  --directory-prefix=/opt/spark/jars/ 

RUN wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar --no-check-certificate  --directory-prefix=/opt/hadoop/share/hadoop/common/ 


RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant vim && \
    apt-get clean;
    
# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f;

RUN chown -R jovyan /opt/spark /opt/hadoop

USER jovyan

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
ENV HADOOP_CONF_DIR /opt/hadoop/etc/hadoop
ENV HADOOP_HOME /opt/hadoop
ENV SPARK_HOME /opt/spark
ENV PATH $PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin:$HADOOP_HOME/bin
ENV PYSPARK_PYTHON /opt/conda/bin/python3

RUN rm -rf /home/jovyan/.cache
