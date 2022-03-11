# Challenge 12 -  K8 Networking - NodePort

**Challenge Type:** Builder


**Goal**: Learn Node Ports
[Context to add]



1.  Deploy a node port service

```json
kubectl apply -f challenge12/simple_nodeport_service.yaml
```

2.  ensure they are all running

```json
kubectl get pods -o wide
```

3.  Let's look at the services

```json
kubectl get services
```

```json
kubectl get services
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.152.183.1     <none>        443/TCP        4h33m
node-svc     NodePort    10.152.183.119   <none>        80:30001/TCP   9m2s
```

4.  We should be able to access the pods behind this node port, get the list of IP's for each node

```json
kubectl get nodes -o wide
```

5.  go into one of the containers

```json
kubectl exec -it challenge12-deployment-6bbfddd8fd-49w6p -- bash
```

6.  Before we ping we need to install ping.

```json
apt-get update
```

```json
apt-get install iputils-ping dnsutils curl iproute2 -y
```

7.  try to curl using one of the node port internal IP addresses

```json
curl 192.168.1.201:30001
```

```json
curl 192.168.1.201:30001
<html>
    <head>
    </head>
    <body>
        <h1>k8_ctf{n0DEP0rt$ARE3VERYwH3r3}</h1>
    </body>
</html>root@networking2-deployment-5cd88f6984-7bgz8:/# exit
```

> 🏁 Challenge 12 Flag - `k8_ctf{n0DEP0rt$ARE3VERYwH3r3}`



exit

```json
exit
```

clean up

```json
kubectl delete -f challenge12/simple_nodeport_service.yaml
```