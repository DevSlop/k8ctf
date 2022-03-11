# Challenge 22 - K8 Security - OPA to Restrict Host Access

**Challenge Type:** Builder

Install OPA
Go into challenge 22 folder

```json
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml
```

Test it out at: [The Rego Playground](https://play.openpolicyagent.org/p/dkoPXIHvTU)

apply the OPA Template and then the constraint

```json
kubectl apply -f opa_policy/template.yaml
kubectl apply -f opa_policy/constraint.yaml
```

then apply a deployment

```json
kubectl apply -f deployment_host_fs.yaml
```

![opa](/screenshots/Pasted%20image%2020220309225311.png)
> 🏁 Challenge 22 Flag - `k8_ctf{poliCyB10cKD3n13D}`




```json
//challenge cleanup
kubectl delete -f deployment_host_fs.yaml
```