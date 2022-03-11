## Challenge 25 - Consume all resources / DoS the Cluster

First lets get metrics on node performance, it may take a few minutes for this to come up

Now, we will Install a cryptominer on our cluster that will consume most of the resources.

```json
kubectl apply -f deployment_benchmark.yaml
```

Try to see what the node performance looks like after the cryptominer is installed

```json
kubectl top nodes
```

![[Pasted image 20220310223959.png]]

We can see that our single pod is consuming 100% of our resources.

<aside> 🚨 Because we are attempting to consume all resources across the entire cluster, the metrics deployment could actually stop working because it ran out of resources. If you cannot get 'kubectl top nodes` to work, its most likely due to this.

</aside>

Pull the log of the pod

```json
kubectl logs [POD NAME]
```

![[Pasted image 20220310224029.png]]

The flag is the name of the cryptominer in the ABOUT section of the logs or the image name of the docker image.

> 🏁 `k8ctf{XMRig}`



Definitely delete this cryptominer as its chewing up all the resources on our cluster

``````
kubectl delete -f deployment_benchmark.yaml
``````