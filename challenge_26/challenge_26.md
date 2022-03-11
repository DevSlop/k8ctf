### Challenge 26 - Fixing Greedy Pods

How do we prevent run away processes that can DOS CPU/Memory?

ensure OPA is installed

```json
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml
```

Open policy agent

[The Rego Playground](https://play.openpolicyagent.org/p/jqcCO9Xv64)

apply the new template and constraint to enforce resource and limits to the cluster

```json
kubectl apply -f template.yaml
```

```json
kubectl apply -f container-resource-limits-constraint.yaml
```

Try to apply the challenge25 file again

```json
kubectl apply -f deployment_benchmark.yaml
```

we should get an OPA error

![[Pasted image 20220310224238.png]]


Flag is:
> 🏁 `k8_ctf{D0n7kilLmYRe5ourcE5}`
