
helm install jupyterhub  jupyterhub/jupyterhub  --namespace dev --values=values.yaml  --debug

 
helm upgrade --install jupyterhub  jupyterhub/jupyterhub  --namespace dev --values=values.yaml --debug


URL

#https://35.247.247.59.nip.io/manjuworks/development/jupyter/user


#Deletion


helm ls

helm delete jupyterhub -n default
