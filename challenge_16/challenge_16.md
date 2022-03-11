# Challenge 16 - K8 Security - Finding Creds in Code	

**Challenge Type:** Breaker

Because kubernetes is a container orchestration framework, many containers will be running on a cluster. Presumably, for these containers to function, they will need to contain creds to access other cloud services or cluster services.

Lets deploy the challenge

```json
kubectl apply -f challenge16/deployment_starwars_creds.yaml
```

You can run to get the load balancer address
```
kubectl get svc
```

Browsing to it is a web app that produces a random starwars quote every time you refresh the page. 
![starwars](/screenshots/Screen%20Shot%202022-03-09%20at%2010.24.52%20PM.png)

Lets go into a pod and browse through the code to find a credential in the deployment.

```json
// get all pods
kubectl get pods

// go into a pod
kubectl exec -it [PODNAME] -- bash

// poke around
cat index.js
```

![credentials](/screenshots/Pasted%20image%2020220309221741.png)


Challenge is the credential.

> 🏁 Challenge 16 Flag - `k8_ctf{09CweCYpHc5tOvwS}`



```json
//exit
exit
```

```json
// clean up deployment
kubectl delete -f challenge16/deployment_starwars_creds.yaml
```