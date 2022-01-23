#https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes
#Following from https://devopscube.com/setup-efk-stack-on-kubernetes/
kubectl run -it --rm --restart=Never debug-container --image=ubuntu bash
apt update -y ;  apt-get install dnsutils  curl telnet -y 
nslookup es-cluster-0.elasticsearch.default.svc.cluster.local

