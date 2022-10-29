
Static Install:

Creates CRDs,Deployments

`kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.9.1/cert-manager.yaml`

`kubectl get all -n cert-manager`

`kubectl apply -f ClusterIssuer.yaml `

`kubectl apply -f  ingress-resource.yaml`
