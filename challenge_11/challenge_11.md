### **Challenge 11 - Cluster IP pinging**

**Challenge Type:** Builder

**Services / Networking**

**GOAL**: Learn how flat a Kubernetes network is.

Find Node IP addresses 

```
kubectl get nodes

NAME                            STATUS   ROLES                  AGE   VERSION
ip-172-20-34-105.ec2.internal   Ready    control-plane,master   17m   v1.20.7
ip-172-20-48-147.ec2.internal   Ready    node                   15m   v1.20.7
```

try running the command below. The `-o` flag is short for `--output` and will include plain-text format with any additional information. F
```
kubectl get nodes -o wide
```

![[Screen Shot 2022-03-09 at 9.30.01 PM.png]]

1.  add a deployment to your cluster with 3 replicas

```json
 kubectl apply -f simple_deployment.yaml
```

2.  ensure it's running

```json
kubectl get pods -o wide
```

You will see that each pod has separate IP addresses
![[Screen Shot 2022-03-09 at 9.36.17 PM.png]]
Validate that the three IP's are coming from the different nodes that have blocks of addresses to give pods.

Find Pod CIDR Ranges

```json
kubectl get nodes -o jsonpath='{.items[*].spec.podCIDR}'
```

Perfect, we see they match.

3.  We can exec onto the pod to test connectivity.
```json
kubectl exec -it nginx-deployment-66b6c48dd5-c4nkl  -- bash
```

4.  Before we ping we need to install ping.

```json
apt-get update

```

```json
apt-get install iputils-ping dnsutils curl iproute2 -y
```

5.  Lets check our Pods IP

```json
ip address
```

(this should match any of the pods you saw before)

6.  Ping the two other hosts. you should be able to ping them no problem.

Access the web servers one of the containers you just pinged to get the flag.

```json
curl [POD.IP.ADD.RESS]

```

```json
root@networking1-7f7476f6f7-cqm7p:/# curl [POD.IP.ADD.RESS]
<html>
    <head>
    </head>
    <body>
        <h1>k8_ctf{cur1iNg_coN7a1nER5}</h1>
    </body>
</html>root@networking1-7f7476f6f7-cqm7p:/# curl 10.1.3.11
<html>
    <head>
    </head>
    <body>
        <h1>k8_ctf{cur1iNg_coN7a1nER5}</h1>
    </body>
```

> 🏁 Challenge 11 Flag - `k8_ctf{cur1iNg_coN7a1nER5}`



Exit

```json
exit
```

cleanup the challenge

```json
kubectl delete -f challenge11/simple_deployment.yaml
```