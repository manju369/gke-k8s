FROM ubuntu:18.04
ENV ZOOKEEPER_USER=zookeeper \
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64  \
JAVA_DIST=openjdk-8-jre-headless \
ZOOKEEPER_LOG_LEVEL=INFO \
ZOOKEEPER_DATA_DIR="/var/lib/zookeeper/data" \
ZOOKEEPER_DATA_LOG_DIR="/var/lib/zookeeper/log" \
ZOOKEEPER_LOG_DIR="/var/log/zookeeper" \
ZOOKEEPER_CONF_DIR="/opt/zookeeper/conf" \
ZOOKEEPER_CLIENT_PORT=2181 \
ZOOKEEPER_SERVER_PORT=2888 \
ZOOKEEPER_ELECTION_PORT=3888 \
ZOOKEEPER_TICK_TIME=2000 \
ZOOKEEPER_INIT_LIMIT=10 \
ZOOKEEPER_SYNC_LIMIT=5 \
ZOOKEEPER_HEAP=1G \
ZOOKEEPER_MAX_CLIENT_CNXNS=60 \
ZOOKEEPER_SNAP_RETAIN_COUNT=3 \
ZOOKEEPER_PURGE_INTERVAL=0 \
ZOOKEEPER_SERVERS=1

ARG ZOOKEEPER_DIST=zookeeper-3.5.9

RUN set -x \
    && apt-get update


RUN set -x \
        && echo "Installing Java" \
        && apt-get install -y  $JAVA_DIST

RUN set -x \
        && apt-get install -y  netcat-openbsd wget gnupg tree

RUN set -x \
        && echo "Downloading ZooKeeper" \
    	  && wget -q "http://www.apache.org/dist/zookeeper/$ZOOKEEPER_DIST/apache-$ZOOKEEPER_DIST-bin.tar.gz" \
        && wget -q "http://www.apache.org/dist/zookeeper/$ZOOKEEPER_DIST/apache-$ZOOKEEPER_DIST-bin.tar.gz.asc"

RUN set -x && wget -q  "http://www.apache.org/dist/zookeeper/KEYS"

RUN set -x && echo "ZooKeeper RESOURCES DOWNLOADED!"

RUN set -x && export GNUPGHOME="$(mktemp -d)" \
            && gpg --import KEYS \
            && gpg --batch --verify "apache-$ZOOKEEPER_DIST-bin.tar.gz.asc" "apache-$ZOOKEEPER_DIST-bin.tar.gz" \
            && tar -xzf "apache-$ZOOKEEPER_DIST-bin.tar.gz" -C /opt \
            && rm -r "$GNUPGHOME" "apache-$ZOOKEEPER_DIST-bin.tar.gz" "apache-$ZOOKEEPER_DIST-bin.tar.gz.asc" KEYS \
            && ln -s /opt/apache-$ZOOKEEPER_DIST-bin /opt/zookeeper

RUN set -x && echo "ZOOKEEPER INSTALLED!"
COPY scripts /opt/zookeeper/bin/

RUN mkdir -p /opt/jmx_exporter/
ADD prom-jmx-agent-config.yml /opt/jmx_exporter/prom-jmx-agent-config.yml
ADD http://x.x.x.x/jmx_prometheus_javaagent-0.3.0.jar /opt/jmx_exporter/jmx_prometheus_javaagent-0.3.0.jar
RUN chmod 777 -R  /opt/jmx_exporter/

COPY java.env /opt/zookeeper/conf/

    # Create a user for the zookeeper process and configure file system ownership
    # for necessary directories and symlink the distribution as a user executable
RUN set -x \
    	&& useradd $ZOOKEEPER_USER \
        && [ `id -u $ZOOKEEPER_USER` -eq 1000 ] \
        && [ `id -g $ZOOKEEPER_USER` -eq 1000 ]

RUN set -x \
      && mkdir -p $ZOOKEEPER_DATA_DIR $ZOOKEEPER_DATA_LOG_DIR $ZOOKEEPER_LOG_DIR /usr/share/zookeeper /tmp/zookeeper /usr/etc/ \
    	&& chown -R "$ZOOKEEPER_USER:$ZOOKEEPER_USER" /opt/apache-$ZOOKEEPER_DIST-bin/ $ZOOKEEPER_DATA_DIR $ZOOKEEPER_LOG_DIR $ZOOKEEPER_DATA_LOG_DIR /tmp/zookeeper /opt/zookeeper \
    	&& ln -s /opt/zookeeper/conf/ /usr/etc/zookeeper \
    	&& ln -s /opt/zookeeper/bin/* /usr/bin \
    	&& ln -s /opt/zookeeper/apache-$ZOOKEEPER_DIST.jar /usr/share/zookeeper/ \
    	&& ln -s /opt/zookeeper/lib/* /usr/share/zookeeper

RUN set -x \
                  && chmod 777 $ZOOKEEPER_DATA_DIR $ZOOKEEPER_LOG_DIR $ZOOKEEPER_DATA_LOG_DIR
RUN set -x \
      &&  chgrp -R 0 "$ZOOKEEPER_DATA_LOG_DIR" "$ZOOKEEPER_DATA_DIR" "$ZOOKEEPER_CONF_DIR" "$ZOOKEEPER_LOG_DIR"  \
                  "/usr/bin/start.sh" "/usr/bin/metrics.sh" "/usr/bin/ready_live.sh"

RUN set -x \
      && chmod -R g=u "$ZOOKEEPER_DATA_LOG_DIR" "$ZOOKEEPER_DATA_DIR" "$ZOOKEEPER_CONF_DIR" "$ZOOKEEPER_LOG_DIR" \
                        "/opt/zookeeper/bin/start.sh" "/opt/zookeeper/bin/metrics.sh" "/opt/zookeeper/bin/ready_live.sh"

RUN set -x \
            && chmod 777 /opt/zookeeper/bin/start.sh

RUN set -x \
            && chmod 777 /opt/zookeeper/bin/metrics.sh

RUN set -x \
            && chmod 777 /opt/zookeeper/bin/ready_live.sh

RUN tree /opt/
