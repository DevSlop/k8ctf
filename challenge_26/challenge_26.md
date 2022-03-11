# Challenge 26 - 	K8 Security - OPA to Enforce Resource Limits

How do we prevent run away processes that can DOS CPU/Memory?

ensure OPA is installed

```json
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml
```

Go to [The Rego Playground](https://play.openpolicyagent.org/p/jqcCO9Xv64) to play around with the policy we are going to add to the cluster to enforce resource and limits to our cluster.

apply the new template and constraint 

```json
kubectl apply -f challenge_26/template.yaml
```

```json
kubectl apply -f challenge_26/container-resource-limits-constraint.yaml
```

Try to apply the challenge25 file again

```json
kubectl apply -f challenge_26/deployment_benchmark_fixed.yaml
```

we should get an OPA error

![opa](/screenshots/Pasted%20image%2020220310224238.png)

Flag is:
> 🏁 `k8_ctf{D0n7kilLmYRe5ourcE5}`
