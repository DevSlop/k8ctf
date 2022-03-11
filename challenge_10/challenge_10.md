## Challenge 10 - K8 Setup - Kubernetes Dashboard

**Challenge Type:** Builder

1.  Apply dashboard to cluster

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

2.   Add service account

```
kubectl create serviceaccount dashboard-admin-sa
```

3.  Bind it to cluster

```
kubectl create clusterrolebinding dashboard-admin-sa --clusterrole=cluster-admin --serviceaccount=default:dashboard-admin-sa
```

4.  get secrets

```
kubectl get secrets
```
(note: everyone's will be different)
![token](/screenshots/Screen%20Shot%202022-03-09%20at%209.19.54%20PM%201.png)

5.  get token

```
kubectl describe secret dashboard-admin-sa-token-[your id]
```

6.  start up kube proxy to access the dashboard

```json
kubectl proxy
```


7.  Log into the [Dashboard](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login) using the link below with the token you just copied:

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```


![dashboard](/screenshots/Screen%20Shot%202022-03-09%20at%209.22.06%20PM.png) 

Poke around the dashboard to get familiar. Go to namespaces, find the namespace that was just created, that is the flag

![dashboard](/screenshots/Screen%20Shot%202022-03-09%20at%209.23.57%20PM.png)

> 🏁 `k8_ctf{kubernetes-dashboard}`

