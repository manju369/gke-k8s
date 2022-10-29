https://medium.com/swlh/set-up-jenkins-in-a-kubernetes-cluster-96660c8d9ab

https://pallavisengupta.medium.com/jenkins-kubernetes-authentication-and-authorization-fa6966356c90

https://medium.com/swlh/set-up-jenkins-in-a-kubernetes-cluster-96660c8d9ab

#To create jenkins agent pods, jenkins master needs this RBAC permission, also set spec.template.spec.serviceAccountName=jenkins in deployment file

`kubectl apply -f rbac.yaml`
