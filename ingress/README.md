#Main Refernce - https://cloud.google.com/community/tutorials/nginx-ingress-gke
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install nginx-ingress ingress-nginx/ingress-nginx

kubectl get deployment nginx-ingress-ingress-nginx-controller

kubectl get service nginx-ingress-ingress-nginx-controller

# Creating external IP might take some time
Create ingress resource now , add the new EXTERNAL IP in ingress-resource file

kubectl apply -f ingress/ingress-resource.yaml  

