# Challenge 29 - 	K8 Security - Using Kube-Bench

**Challenge Type:** Builder

Kube-bench is a tool to check for cluster misconfigurations.

apply it

```json
kubectl apply -f <https://raw.githubusercontent.com/aquasecurity/kube-bench/main/job.yaml>
```

```json
kubectl get pods
```

```json
kubectl logs kube-bench-59rm8
```

This will show you best practices configurations for you cluster

Flag is the total number of checks PASSED

> 🏁 `k8_ctf{19}`

