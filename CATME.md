# HOW-TO Deploy Minikube Apps

### Start minikube
You'll want to start minikube and make sure the ingress and ingress-dns controller addons are enabled/running: alias "minikube_destroy"
Not sure if it matters, but bring up the minikube tunnel: alias "minikube_start"


### Deploying a simple app like minikube-nginx-ingress
Run the commands in order from that directory
```
1. kubectl run minikube-deployment --image=gcr.io/google_containers/echoserver:1.4 --port=8080
2. kubectl create –f .
3. minikube service --namespace=<namespace> <name-of-service> --url
```


##### Deploying the minikube dashboard
Run the commands in order from that directory
---
```
1. kubectl apply -f k8s-dashboard-minik8s-deployment.yaml
2. kubectl -n kubernetes-dashboard patch svc kubernetes-dashboard -p '{"spec": {"type": "NodePort"}}'
3. kubectl -n kubernetes-dashboard patch svc kubernetes-dashboard -p "$(cat nodeport_dashboard_patch.yaml)"
```

----(as admin)----
```
SA_NAME="monash"
kubectl apply -f admin-sa.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep ${SA_NAME} | awk '{print $1}')
```

----(as user)----
```
NAMESPACE="kubernetes-dashboard"
K8S_USER="user"
kubectl apply -f user-sa.yaml
kubectl -n ${NAMESPACE} describe secret $(kubectl -n ${NAMESPACE} get secret | (grep ${K8S_USER} || echo "$_") | awk '{print $1}') | grep token: | awk '{print $2}'\n
```