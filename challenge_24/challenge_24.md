# Challenge 24 - 	K8 Security - Prevent Namespace Bypass w/ Cilium

**Challenge Type:** Builder

Lets fix the issue in Challenge 23

**What is Cilium?**

Cilium is open source software for transparently securing the network connectivity between application services deployed using Linux container management platforms like Docker and Kubernetes.

Let's run through an example:

In our cluster run these commands:

```json
kubectl create ns cilium-test
```

```json
kubectl apply -n cilium-test -f <https://raw.githubusercontent.com/cilium/cilium/v1.8/examples/kubernetes/connectivity-check/connectivity-check.yaml>
```

```json
kubectl port-forward -n kube-system svc/hubble-ui --address 0.0.0.0 --address :: 12000:80

```

Browse to [http://localhost:12000/cilium-test](http://localhost:12000/cilium-test)

this is a test namespace where you can observe traffic flowing through pods.

Our goal is to create a network policy so that no one on the cluster can access MongoDB ports (27017, 28017)

We can visualize this here.
[Network Policy Editor for Kubernetes](https://editor.cilium.io/)

We want to create a network policy that disables access from anywhere in the cluster to port 27017,28017.

![cilium](/screenshots/Pasted%20image%2020220310213735.png)
Once you have your policy, apply it to the cluster

```json
kubectl apply -f networkpolicy.yaml
```

Retry accessing the mongodb environment.

go back into the pentest tools pod

```json
kubectl exec -it [PODNAME]-- bash
```

```json
root@challenge23-attacker-8bd985cb5-lk6dt:/app# curl http://[MONGOIP]:28017/k8ctf/flag/
```

the network policy should be blocking our ability to get to the page. 


The namespace that we are restricting is the flag for this challenge
> 🏁 `k8_ctf{admin}`



To clean up you can remove the network policy

```json
kubectl delete -f networkpolicy.yaml
kubectl delete -f 
```