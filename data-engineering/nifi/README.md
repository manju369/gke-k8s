# nifi-kubernetes-aks
To set-up Nifi with Statefulset


Nifi Runs on port 9080 here , also binds with all network interfaces ..i.e 0.0.0.0


To test:
spin-up a debugging container

kubectl run -it --rm --restart=Never debug-container --image=ubuntu bash

root@debug-container:/# apt update ; apt-get install dnsutils ;  apt-get install curl telnet

root@debug-container:/# nslookup nifi-service.default.svc.cluster.local

root@debug-container:/# telnet  nifi-service.default.svc.cluster.local 9080

root@debug-container:/# curl nifi-service.default.svc.cluster.local:9080
