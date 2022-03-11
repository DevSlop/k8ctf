### **Challenge 13 - LoadBalancer**

**Challenge Type:** Builder

[context explain loadbalancer]

1.  Deploy a load balancer service

```json
kubectl apply -f challenge13/simple_load_balancer.yaml
```

2.  ensure they are all running

```json
kubectl get pods -o wide
```

Get Services

```json
kubectl get services
```

3.  Get endpoints

```json
kubectl get endpoints challenge13
```

4.  Lets look at the services

```json
kubectl get svc -o wide

```

if you see `pending` thats ok, it can take a bit for AWS to spin it up
```json
kubectl get svc -o wide
NAME          TYPE           CLUSTER-IP      EXTERNAL-IP     PORT(S)        AGE     SELECTOR
kubernetes    ClusterIP      10.152.183.1    <none>          443/TCP        6h36m   <none>
challenge13   LoadBalancer   10.152.183.13   a5bfe76ee236949bab3a0edb8ac6fd18-235417961.us-east-1.elb.amazonaws.com   80:31636/TCP   3s      app=networking3
```

5. Using your browser type in the IP of the AWS Load balancer

![[Screen Shot 2022-03-09 at 9.59.24 PM.png]]

> 🏁 Challenge 13 Flag - `k8_ctf{LoADbAlaNCERSfe3LSE3n}`


clean up

```json
kubectl apply -f challenge13/simple_load_balancer.yaml
```