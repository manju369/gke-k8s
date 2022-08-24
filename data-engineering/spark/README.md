
```helm install spark-operator-release  spark-operator/spark-operator --namespace default --dry-run --debug  > dry-run.out ```

``` helm upgrade --install spark-operator-release  spark-operator/spark-operator --namespace default --debug --set sparkJobNamespace=default --set logLevel=10 --set image.tag=v1beta2-1.3.7-3.1.1 --set serviceAccounts.spark.name=spark ```


```helm upgrade --install spark-operator-release  spark-operator/spark-operator --namespace spark-operator  --set sparkJobNamespace=spark-operator --set enableWebhook=true --set webhookPort=443 --set enableMetrics=true --set logLevel=10 --set image.tag=v1beta2-1.3.7-3.1.1 --set serviceAccounts.spark.name=spark --debug ```


```kubectl get  sparkapplication --namespace default ```


Create Kubernetes secrets to store GCS credentials:

```kubectl create secret generic gcs-bq  --from-file=key.json=/Users/manjunath/Downloads/steam-circlet-330806-43e42eeeb5ff.json```


Spark Job Commands: 


```kubectl describe sparkapplications.sparkoperator.k8s.io spark-pi --namespace default```

```kubectl port-forward svc/spark-pi-ui-svc 4040:4040 --namespace default ```


Sparkctl commands:

```sparkctl list --namespace default```

```sparkctl status spark-pi --namespace default```

```sparkctl log spark-pi --namespace default```

```sparkctl forward spark-pi --namespace default```

Spark Downloads: https://archive.apache.org/dist/spark/


Webhooks:

* A lot of changes need to be done in spark-operator/manifests/spark-operator-with-webhook.yaml 
  especially on hard-coded namespaces https://yinxu4876455.medium.com/enable-the-webhook-for-spark-operator-4700fde5ab32

* 
```kubectl create clusterrole secretadmin  --verb=get --verb=list --verb=create --verb=update --resource=secret --namespace=default


   kubectl create clusterrolebinding canadminsecret --clusterrole=secretadmin --serviceaccount=default:spark  ```


* k apply -f spark-operator/manifests/spark-operator-with-webhook.yaml 
