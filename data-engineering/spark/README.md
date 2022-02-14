helm install spark-operator-release  spark-operator/spark-operator --namespace default --dry-run --debug  > dry-run.out

helm upgrade --install spark-operator-release  spark-operator/spark-operator --namespace default --debug --set sparkJobNamespace=default --set image.tag=v1beta2-1.3.0-3.1.1 --set serviceAccounts.spark.name=spark

kubectl get  sparkapplication



Create Kubernetes secrets to store GCS credentials

#kubectl create secret generic gcs-bq  --from-file=key.json=/Users/manjunath/Downloads/steam-circlet-330806-43e42eeeb5ff.json

