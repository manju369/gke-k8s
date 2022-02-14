#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
ARG ROOT_CONTAINER=ubuntu:focal

FROM ${ROOT_CONTAINER}

ARG openjdk_version="8"
ARG spark_uid=1000

# Before building the docker image, first build and make a Spark distribution following
# the instructions in http://spark.apache.org/docs/latest/building-spark.html.
# If this docker file is being used in the context of building your images from a Spark
# distribution, the docker build command should be invoked from the top level directory
# of the Spark distribution. E.g.:
# docker build -t spark:latest -f kubernetes/dockerfiles/spark/Dockerfile .
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    "openjdk-${openjdk_version}-jre-headless" \
    ca-certificates-java 

RUN apt-get install --yes software-properties-common 
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get update && apt-get install -y \
        python3.7 \
        python3-pip \
        python3-distutils \
        python3-setuptools
RUN pip install pyspark==3.2.0

RUN set -ex && \
    sed -i 's/http:\/\/deb.\(.*\)/https:\/\/deb.\1/g' /etc/apt/sources.list && \
    apt-get update && \
    ln -s /lib /lib64 && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt install -y -qq bash tini libc6 libpam-modules krb5-user libnss3 procps  && \
    mkdir -p /opt/spark && \
    mkdir -p /opt/spark/examples && \
    mkdir -p /opt/spark/work-dir && \
    mkdir -p /etc/metrics/conf/ && \
    mkdir -p /opt/hadoop/ && \
    touch /opt/spark/RELEASE && \
    rm /bin/sh && \
    ln -sv /bin/bash /bin/sh && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    chgrp root /etc/passwd && chmod ug+rw /etc/passwd && \
    apt-get clean && rm -rf /var/lib/apt/lists/* \
    rm -rf /var/cache/apt/* 

COPY jars /opt/spark/jars
COPY bin /opt/spark/bin
COPY sbin /opt/spark/sbin
COPY kubernetes/dockerfiles/spark/entrypoint.sh /opt/
COPY kubernetes/dockerfiles/spark/decom.sh /opt/
COPY examples /opt/spark/examples
COPY kubernetes/tests /opt/spark/tests
COPY data /opt/spark/data
COPY conf/prometheus.yaml /etc/metrics/conf/
COPY hadoop /opt/hadoop

RUN ls -la /opt/spark/jars | grep jmx
RUN ls -la /etc/metrics/conf/
RUN ls -la /opt/hadoop/

ENV SPARK_HOME /opt/spark
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
WORKDIR /opt/spark/work-dir
RUN chmod g+w /opt/spark/work-dir
RUN chmod a+x /opt/decom.sh

RUN  mkdir -p /opt/spark/logs && \
     chown -R 1000:1000 /opt/spark/logs

ENTRYPOINT [ "/opt/entrypoint.sh" ]
# Specify the User that the actual main process will run as
USER ${spark_uid}
