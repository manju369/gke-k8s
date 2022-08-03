#Main Refernce - https://cloud.google.com/community/tutorials/nginx-ingress-gke
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update

helm install nginx-ingress ingress-nginx/ingress-nginx

kubectl get deployment nginx-ingress-ingress-nginx-controller

kubectl get service nginx-ingress-ingress-nginx-controller
kubectl --namespace default get services -o wide -w nginx-ingress-ingress-nginx-controller

# Creating external IP might take some time
Create ingress resource now , add the new EXTERNAL IP in ingress-resource file

kubectl apply -f ingress/ingress-resource.yaml  

##TLS

openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout ingress-tls.key -out ingress-tls.crt -subj "/CN=34.172.201.69.nip.io" -days 365


kubectl create secret tls 34.172.201.69.nip.io-tls --cert=ingress-tls.crt --key=ingress-tls.key
