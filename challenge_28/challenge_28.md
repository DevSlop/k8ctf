###  Challenge 29 - OPA Rules to restrict image origin

**Challenge Type:** Builder

You can use OPA to restrict image sources

Ensure OPA is installed

```json
kubectl apply -f <https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml>
```

[The Rego Playground](https://play.openpolicyagent.org/p/hsAhGBE2ar)

apply the template

```json
kubectl apply -f template.yaml
```

apply the constraint

```json
kubectl apply -f constraint.yaml
```

deploy the crypto miner

```json
kubectl apply -f deployment_benchmark.yaml
```

> 🏁 `k8_ctf{n0wEiRDcoN7AInErSoNmYc1u$7ER}`
