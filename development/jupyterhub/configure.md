Configure to use GCS buckets 

https://github.com/GoogleCloudDataproc/hadoop-connectors/blob/master/gcs/INSTALL.md
In python notebook create credentials file in `/tmp/key.json`. Later to be made it as kubernetes secrets

```
credentials=r"""{
    "type": "service_account",
    "project_id": "arctic-surf-351605",
    "private_key_id": "8af25830492c8d13e3a9a7a946d27f5872713f14",
    "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDV5CLaOvemHYrW\nhe7iubROJLzMi7E57dJZsdCAPp62Lr22QUKQPmoVSsANUg1tiLreTXdwxhwaEg60\nh0g41hy8op0gGfZrz3inJFL7+Aw2Wi4HQg+OuCyZTeHLVcjBI4YeNZLfrjl023Uh\nM+FBYY3lgI7s8y3BUQS4tAEvpCF8E+mFEAM8ItOgJJRxN0GO9CHuj8ZnurQt9Pvq\nxXmRvuiGQQ65fxMI53ACHAXa/BWfNXzUnzLo1IcSkZtmMfaBdWVO8RdMJYi9n9FG\nGxQ/bfC57XwdKMfdAUdFK8VInZubwkHVuznlw3vzlDpsgFTw2jbRzm8YuRHs103E\nMnEgx44TAgMBAAECggEANm4LVniaC9pJugS0QVsboKUlrHHCBGlaVbvzwKbu0ZK8\ncVRTIYzYhxmSHPGr/BtG3opdIkQv44yD1Hn2rYwDHRfOn1wsAsx1uib6vScmyHAT\ndYJgniONKegRVSK75zZoi2h7u1NFSNEOEgh3QqGWL/iSpobNtnutFWmtQZflc4LQ\nmOGG0l5rYBm8lWpPs2GEfFUOtOKMMAf493wjPR21ZjFZUpGrH4l33Eykx137wPmz\nsAtLIdoq8eGuTmb2LML4jQOFWU/8UzKeDR+U5BmGy4pj1N5xF6b/QS3qglnHDawo\noh/1XFcSPIoZ7aUESJ40tyqbdum/UOjLp3YtWb+VMQKBgQDsFtLkcy8qrayQgdO6\nJuaKajB2bNMbiCwwrlqcOwuhGLSo3ijtFSSsVm2svKruUl05ldrcp8r288iCXAvs\nYC2S6h1YOrJdAmmsduDoxByM2SXsULQmew9l71aRhMPRcmkPT7QzUT9Id8dubC2y\nLA8D7s8JH1buZ5Zn6o9n1l3kYwKBgQDn7g5186D7IX/jGV1y06k8R2LRvg2j4+k+\nbk+jTdQsg19SgmQVR2bTsxpLw4SBUi35hd5YRyw5963u3Tr14UDdDf+mR8qgHroB\nG/ukLRhxdECFmDcbrL5/WIBIcfJAXXlODqA6ETSn9y2kArvMx5hki8YcK8JFvbGe\nqRRkM8umkQKBgQCO5UWiVoFe8J25HYt8aZ0yfBF3LGkeie5NTMq1MxvX1u9KIqVc\n0mZPFUTuv4cqFposh6Jf2gOEM4vSM0pYOOJ8wM0gIO7iUtqJM67v0/t/2NMWVMal\nX+izBwk7rMMlG32xccmdIfkOsMj58eo6pGY2OC4B1IE2bWZg4V0JOXu9BwKBgQDY\nn8Tp7nABn7xvRRW/VmrPXm2yMa1T0l/ca/P+N4dhPCMgUaFhLpugR7zb3vY4Q6Wl\nVZ/jHDb2vZu7au7TEV0gGx8ZFBzxiUF6H8TeBzC5ZzeMkCuIscQL9YF8KNF3xNa0\nTXziP4fLCleaxOLb1eFEqDiVv1lpAlCQPKRLIwWnwQKBgF/fO5fOjW0aabcxCLGe\nF5BG+5TvozaqrgXE20OebVUUf4TVnH5jwv0pbIEupiaIMzlI9PXhWiiSnc3v0obw\n0spPV3CmQtaawyJBuTvs+m0b6q/BszYuUeZ86MQWQqpZPbxbsqZCpRcMwHKqNOoS\nwhPbk0uIFjKs561zx49aRZux\n-----END PRIVATE KEY-----\n",
    "client_email": "gcs-k8s-spark@arctic-surf-351605.iam.gserviceaccount.com",
    "client_id": "112980677907214490104",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/gcs-k8s-spark%40arctic-surf-351605.iam.gserviceaccount.com"
  }"""
f = open("/tmp/key.json", "w")
f.write(credentials)
f.close()
```
core-site.xml file updation

```
core_site="""<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>

<property>
  <name>fs.AbstractFileSystem.gs.impl</name>
  <value>com.google.cloud.hadoop.fs.gcs.GoogleHadoopFS</value>
  <description>The AbstractFileSystem for 'gs:' URIs.</description>
</property>

<property>
  <name>fs.gs.project.id</name>
  <value></value>
  <description>
    Optional. Google Cloud Project ID with access to GCS buckets.
    Required only for list buckets and create bucket operations.
  </description>
</property>

<property>
  <name>google.cloud.auth.type</name>
  <value>SERVICE_ACCOUNT_JSON_KEYFILE</value>
  <description>
    Authentication type to use for GCS access.
  </description>
</property>

<property>
  <name>google.cloud.auth.service.account.json.keyfile</name>
  <value>/tmp/key.json</value>
  <description>
    The JSON keyfile of the service account used for GCS
    access when google.cloud.auth.type is SERVICE_ACCOUNT_JSON_KEYFILE.
  </description>
</property>

</configuration>"""

f = open("/opt/hadoop/etc/hadoop/core-site.xml", "w")
f.write(core_site)
f.close()

```

Download GCS jar 

```
!cd /opt/hadoop/share/common; wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar
!cd /opt/spark/jars/; wget https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar
```

HDFS command execution 

```!JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 /opt/hadoop/bin/hadoop --loglevel TRACE  fs  -ls gs://steam-circlet-first-bucket/titanic/ingestion/test.csv
```


Spark Configurations:


```
#! pip install pyspark==3.2.0 

from pyspark.sql import SparkSession
spark = SparkSession.builder.appName('tutorial').getOrCreate()

spark.conf.set("google.cloud.auth.service.account.enable", "true")
spark.conf.set("google.cloud.auth.service.account.json.keyfile", "/tmp/key.json")


df_train = spark.read.csv('gs://steam-circlet-first-bucket/titanic/ingestion/test.csv', header = True, inferSchema=True)
df_train.show()
```
