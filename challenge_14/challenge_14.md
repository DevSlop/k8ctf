# Challenge 14 - K8 Deployment - Simple Deployment

**Challenge Type:** Builder

[add context about rollling updates]

Lets learn about rolling updates and why this is cool on kubernetes.

First let's deploy something

```json
kubectl apply -f 1-deployment_rolling_update_replicaset.yaml
```

This will deploy a nginx server with 3 replica's and a load balancer.

Find the address of the LB

```json
kubectl get svc
```

go to the load balancer url / ip.

we gotta update and expand! Next challenge

![load balancer](/screenshots/Screen%20Shot%202022-03-09%20at%2010.06.11%20PM.png)


> 🏁 Challenge 14 Flag - `k8_ctf{bl0s50m1n9dEPLOymen7}`



no cleanup yet