helm repo add apache-airflow https://airflow.apache.org

helm repo update

helm upgrade --install airflow apache-airflow/airflow -n default -f values.yaml

helm show values apache-airflow/airflow > values.yaml

#Logs PVC

kubectl apply -f  airflow-logs-storage-class.yml 

kubectl apply -f airflow-logs-pvc.yml

#ssh-key shouldn't be encoded manually

kubectl create secret generic airflow-ssh-secret --from-file=gitSshKey=/Users/admin/.ssh/id_rsa -n data-eng

#airflow-worker service-account wont be having permission to create object in sparkoperator.k8s.io by default so , execute below CRB  
kubectl apply -f airflow-worker-spark-service-account.yml 




kubectl exec -it airflow-scheduler-86d8459b64-th6rx -c scheduler /bin/bash #OR

kubectl exec -it airflow-scheduler-86d8459b64-th6rx -c scheduler /bin/ls  dags/

kubectl logs airflow-scheduler-86d8459b64-th6rx -c git-sync



#Open source docs
https://airflow.apache.org/docs/helm-chart/stable/production-guide.html#production-guide-knownhosts
https://airflow.apache.org/docs/docker-stack/build.html
https://airflow.apache.org/docs/helm-chart/stable/manage-logs.html
https://artifacthub.io/packages/helm/airflow-helm/airflow
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
<!--
 Licensed to the Apache Software Foundation (ASF) under one
 or more contributor license agreements.  See the NOTICE file
 distributed with this work for additional information
 regarding copyright ownership.  The ASF licenses this file
 to you under the Apache License, Version 2.0 (the
 "License"); you may not use this file except in compliance
 with the License.  You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing,
 software distributed under the License is distributed on an
 "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 KIND, either express or implied.  See the License for the
 specific language governing permissions and limitations
 under the License.
 -->

# Helm Chart for Apache Airflow

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/apache-airflow)](https://artifacthub.io/packages/search?repo=apache-airflow)

[Apache Airflow](https://airflow.apache.org/) is a platform to programmatically author, schedule and monitor workflows.

## Introduction

This chart will bootstrap an [Airflow](https://airflow.apache.org) deployment on a [Kubernetes](http://kubernetes.io)
cluster using the [Helm](https://helm.sh) package manager.

## Requirements

- Kubernetes 1.14+ cluster
- Helm 3.0+
- PV provisioner support in the underlying infrastructure (optionally)

## Features

* Supported executors: ``LocalExecutor``, ``CeleryExecutor``, ``CeleryKubernetesExecutor``, ``KubernetesExecutor``.
* Supported Airflow version: ``1.10+``, ``2.0+``
* Supported database backend: ``PostgresSQL``, ``MySQL``
* Autoscaling for ``CeleryExecutor`` provided by KEDA
* PostgresSQL and PgBouncer with a battle-tested configuration
* Monitoring:
   * StatsD/Prometheus metrics for Airflow
   * Prometheus metrics for PgBouncer
   * Flower
* Automatic database migration after a new deployment
* Administrator account creation during deployment
* Kerberos secure configuration
* One-command deployment for any type of executor. You don't need to provide other services e.g. Redis/Database to test the Airflow.

## Documentation

Full documentation for Helm Chart (latest **stable** release) lives [on the website](https://airflow.apache.org/docs/helm-chart/).

> Note: If you're looking for documentation for main branch (latest development branch): you can find it on [s.apache.org/airflow-docs/](http://apache-airflow-docs.s3-website.eu-central-1.amazonaws.com/docs/helm-chart/latest/index.html).
> Source code for documentation is in [../docs/helm-chart](https://github.com/apache/airflow/tree/main/docs/helm-chart)
>

## Contributing

Want to help build Apache Airflow? Check out our [contributing documentation](https://github.com/apache/airflow/blob/main/CONTRIBUTING.rst).
