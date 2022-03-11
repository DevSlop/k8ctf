### Challenge 10 - Deploy Kubernetes Dashboard

1.  Apply dashboard to cluster

```json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

2.   Add service account

```json
kubectl create serviceaccount dashboard-admin-sa
```

3.  Bind it to cluster

```json
kubectl create clusterrolebinding dashboard-admin-sa --clusterrole=cluster-admin --serviceaccount=default:dashboard-admin-sa
```

4.  get secrets

```json
kubectl get secrets
```
(note: everyone's will be different)
![[Screen Shot 2022-03-09 at 9.19.54 PM 1.png]]
5.  get token

```json
kubectl describe secret dashboard-admin-sa-token-[your id]
```

6.  start up kube proxy to access the dashboard

```json
kubectl proxy
```


7.  Log into the dashboard with the token:

[Dashboard](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login)

```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```


![[../images/Screen Shot 2022-03-09 at 9.22.19 PM.png]]

Poke around the dashboard to get familiar. Go to namespaces, find the namespace that was just created, that is the flag
![[../images/Screen Shot 2022-03-09 at 9.23.57 PM.png]]

> 🏁 `k8_ctf{kubernetes-dashboard}`

