#Download spark tarball with specfic version 
# Download GCS jars in spark library in $SPARK_HOME/jars/ https://storage.googleapis.com/hadoop-lib/gcs/gcs-connector-hadoop3-latest.jar also maybe https://repo1.maven.org/maven2/com/google/cloud/bigdataoss/gcs-connector/hadoop3-2.2.4/gcs-connector-hadoop3-2.2.4-shaded.jar 

# cd ~/spark/spark-3.2.0-bin-hadoop3.2/ ; sudo  ./bin/docker-image-tool.sh -r manju369/spark-3.2.0-bin-hadoop3.2-gcs -t 2  build
# sudo docker tag manju369/spark-3.2.0-bin-hadoop3.2-gcs/spark:2  manju369/spark-3.2.0-bin-hadoop3.2-gcs:2
# sudo docker push manju369/spark-3.2.0-bin-hadoop3.2-gcs:2

Create Kubernetes secrets to store GCS credentials
#kubectl create secret generic gcs-bq  --from-file=key.json=/Users/manjunath/Downloads/steam-circlet-330806-43e42eeeb5ff.json 


