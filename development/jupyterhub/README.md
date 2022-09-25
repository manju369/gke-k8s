helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
helm install jupyterhub  jupyterhub/jupyterhub  --namespace dev --values=values.yaml  --debug

 
helm upgrade --install jupyterhub  jupyterhub/jupyterhub  --namespace dev --values=values.yaml --debug
kubectl create secret generic gcs-bq  --from-file=key.json=/Users/admin/Downloads/my-test-project-361204-1f57930f6914.json  -n dev

URL

#https://35.247.247.59.nip.io/manjuworks/development/jupyter/user


#Deletion


helm ls

helm delete jupyterhub -n default
