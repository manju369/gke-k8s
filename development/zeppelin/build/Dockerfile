#https://github.com/apache/zeppelin/blob/master/scripts/docker/zeppelin/bin/Dockerfile
FROM ubuntu:20.04
#FROM jupyter/base-notebook:ubuntu-20.04

LABEL maintainer="Apache Software Foundation <dev@zeppelin.apache.org>"

ENV Z_VERSION="0.10.1"

ENV LOG_TAG="[ZEPPELIN_${Z_VERSION}]:" \
    ZEPPELIN_HOME="/opt/zeppelin" \
    HOME="/opt/zeppelin" \
    LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    ZEPPELIN_ADDR="0.0.0.0"

RUN echo "$LOG_TAG install basic packages" && \
    apt-get -y update && \
    # Switch back to install JRE instead of JDK when moving to JDK9 or later.
    DEBIAN_FRONTEND=noninteractive apt-get install -y locales language-pack-en tini vim openjdk-8-jdk-headless wget unzip && \
    # Cleanup
    rm -rf /var/lib/apt/lists/* && \
    apt-get autoclean && \
    apt-get clean

# Install conda to manage python and R packages
ARG miniconda_version="py37_4.9.2"
# Hashes via https://docs.conda.io/en/latest/miniconda_hashes.html
ARG miniconda_sha256="79510c6e7bd9e012856e25dcb21b3e093aa4ac8113d9aa7e82a86987eabe1c31"
# Install python and R packages via conda
COPY env_python_3_with_R.yml /env_python_3_with_R.yml

RUN set -ex && \
    wget -nv https://repo.anaconda.com/miniconda/Miniconda3-${miniconda_version}-Linux-x86_64.sh -O miniconda.sh && \
    echo "${miniconda_sha256} miniconda.sh" > anaconda.sha256 && \
    sha256sum --strict -c anaconda.sha256 && \
    bash miniconda.sh -b -p /opt/conda && \
    export PATH=/opt/conda/bin:$PATH && \
    conda config --set always_yes yes --set changeps1 no && \
    conda info -a && \
    conda install -c conda-forge mamba
    
RUN /opt/conda/bin/mamba env update -f /env_python_3_with_R.yml --prune 
    # Cleanup
RUN rm -v miniconda.sh anaconda.sha256  && \
    # Cleanup based on https://github.com/ContinuumIO/docker-images/commit/cac3352bf21a26fa0b97925b578fb24a0fe8c383
    find /opt/conda/ -follow -type f -name '*.a' -delete && \
    find /opt/conda/ -follow -type f -name '*.js.map' -delete && \
    /opt/conda/bin/mamba clean -ay
    # Allow to modify conda packages. This allows malicious code to be injected into other interpreter sessions, therefore it is disabled by default
    # chmod -R ug+rwX /opt/conda
ENV PATH /opt/conda/envs/python_3_with_R/bin:/opt/conda/bin:$PATH

RUN echo "$LOG_TAG Download Zeppelin binary" && \
    mkdir -p ${ZEPPELIN_HOME} && \
    wget -nv -O /tmp/zeppelin-${Z_VERSION}-bin-all.tgz https://archive.apache.org/dist/zeppelin/zeppelin-${Z_VERSION}/zeppelin-${Z_VERSION}-bin-all.tgz && \
    tar --strip-components=1 -zxvf  /tmp/zeppelin-${Z_VERSION}-bin-all.tgz -C ${ZEPPELIN_HOME} && \
    rm -f /tmp/zeppelin-${Z_VERSION}-bin-all.tgz && \
    chown -R root:root ${ZEPPELIN_HOME} && \
    mkdir -p ${ZEPPELIN_HOME}/logs ${ZEPPELIN_HOME}/run ${ZEPPELIN_HOME}/webapps && \
    # Allow process to edit /etc/passwd, to create a user entry for zeppelin
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd && \
    # Give access to some specific folders
    chmod -R 775 "${ZEPPELIN_HOME}/logs" "${ZEPPELIN_HOME}/run" "${ZEPPELIN_HOME}/notebook" "${ZEPPELIN_HOME}/conf" && \
    # Allow process to create new folders (e.g. webapps)
    chmod 775 ${ZEPPELIN_HOME} && \
    chmod -R 775 /opt/conda

COPY log4j.properties ${ZEPPELIN_HOME}/conf/
COPY log4j_docker.properties ${ZEPPELIN_HOME}/conf/
COPY log4j2.properties ${ZEPPELIN_HOME}/conf/
COPY log4j2_docker.properties ${ZEPPELIN_HOME}/conf/


RUN mkdir /opt/spark /opt/hadoop

RUN wget https://archive.apache.org/dist/spark/spark-3.2.0/spark-3.2.0-bin-hadoop3.2.tgz --no-check-certificate  --directory-prefix=/tmp/ && tar -xvzf /tmp/spark-3.2.0-bin-hadoop3.2.tgz -C /opt/spark --strip-components 1  && rm /tmp/*.tgz

RUN wget https://archive.apache.org/dist/hadoop/common/hadoop-3.2.0/hadoop-3.2.0.tar.gz  --no-check-certificate  --directory-prefix=/tmp/ &&  tar -xvzf /tmp/hadoop-3.2.0.tar.gz -C /opt/hadoop --strip-components 1 && rm  /tmp/*.gz


RUN wget https://repo1.maven.org/maven2/io/prometheus/jmx/jmx_prometheus_javaagent/0.11.0/jmx_prometheus_javaagent-0.11.0.jar  --no-check-certificate  --directory-prefix=/opt/spark/jars/

RUN wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar  --no-check-certificate  --directory-prefix=/opt/spark/jars/

RUN wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar --no-check-certificate  --directory-prefix=/opt/hadoop/share/hadoop/common/

RUN chown -R 1000:1000 /opt/spark /opt/hadoop
USER 1000

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
ENV HADOOP_CONF_DIR /opt/hadoop/etc/hadoop
ENV HADOOP_HOME /opt/hadoop
ENV SPARK_HOME /opt/spark
ENV PYSPARK_PYTHON /opt/conda/bin/python3
ENV PATH $PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin:$HADOOP_HOME/bin

EXPOSE 8080

ENTRYPOINT [ "/usr/bin/tini", "--" ]
WORKDIR ${ZEPPELIN_HOME}
CMD ["bin/zeppelin.sh"]
