helm install spark-operator-release  spark-operator/spark-operator --namespace default --dry-run --debug  > dry-run.out
helm upgrade --install spark-operator-release  spark-operator/spark-operator --namespace default --set sparkJobNamespace=default --set serviceAccounts.spark.name=spark
kubectl get  sparkapplication
