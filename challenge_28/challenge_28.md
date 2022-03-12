#  Challenge 28- K8 Security - OPA to Enforce Trusted Image Sources	

**Challenge Type:** Builder

You can use OPA to restrict image sources, in the case below we will only allow gcr.io

Our rego should look like this:  

```r
allowlisted_registries = {registry |
                 registry = 
                            [
                            "gcr.io"
                            ]
        }
        registry_allowlisted(str, patterns) {
            registry_matches(str, patterns[_])
        }
        registry_matches(str, pattern) {
            contains(str, pattern[_])
        }
        violation[{"msg": msg}] {
         
          image := input.request.spec.template.spec.containers[_].image
          name := input.request.metadata.name
          not registry_allowlisted(image, allowlisted_registries)
          
          msg := sprintf("policy requires pod %q to have a valid Doc registry %q. \nTo comply with security policies registry should be one of %v", [name, image, allowlisted_registries])
        }
```

Play around with it at the [The Rego Playground](https://play.openpolicyagent.org/p/Xl2uyTBFlw)

```json
//apply the template
kubectl apply -f template.yaml
```


```json
//apply the constraint
kubectl apply -f constraint.yaml
```


```json
//try re-deploy the crypto miner, this time any images from docker.io will not be allowed
kubectl apply -f challenge_26/deployment_benchmark_fixed.yaml
```

> 🏁 `k8_ctf{n0wEiRDcoN7AInErSoNmYc1u$7ER}`
