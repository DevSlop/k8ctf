# Challenge 22 - K8 Security - OPA to Restrict Host Access

**Challenge Type:** Builder

Install OPA to our cluster
```json
kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml
```

Our Rego should look like below:

We are explicity allowing host paths `/foo`, `/app` and `/dir`. If our deployment wants to mount `/` this is not on the list and should be rejected.
```ruby

        violation[{"msg": msg, "details": {}}] {
            # list of all our allowed paths
            allowedpaths := paths[_] 
           
            # get the host path from the yaml/json
            hostpath := input.request.spec.template.spec.volumes[_].hostPath.path
            
            # evaluate if the yaml's hostpath is in the allowed list
            not contains(allowedpaths, hostpath)
            msg := sprintf("%v not in allowed paths", [hostpath])
        }

		# function to evaluate
		contains(allowedpaths, hostpath) {
                hostpath == allowedpaths[_]
        }
        
        paths = {path | path = 
            			[
            			"/foo",
                        "/app",
                        "/dir"
                        ]
            	}
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