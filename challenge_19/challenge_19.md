
## Challenge 19 - Finding / Fixing SSRF in K8

**Challenge Type:** Builder

Mitigating SSRF in K8.

[Privilege Escalation in AWS Elastic Kubernetes Service (EKS) by compromising the instance role of worker nodes - Christophe Tafani-Dereeper](https://blog.christophetd.fr/privilege-escalation-in-aws-elastic-kubernetes-service-eks-by-compromising-the-instance-role-of-worker-nodes/)

Christophe has a blog post detailing how difficult it is to mitigate these types of vulnerabilities:

This flaw is actually not new – it’s even [documented](https://docs.aws.amazon.com/eks/latest/userguide/restrict-ec2-credential-access.html) by AWS, who suggests to block the Instance Metadata service by creating an iptables rule on every EKS worker node. This is rather impractical if your EKS nodes are managed by [node groups](https://docs.aws.amazon.com/eks/latest/userguide/managed-node-groups.html), dynamically spinning up EC2 instances to act as EKS worker nodes. You’d need to create a [custom node group launch template](https://aws.amazon.com/blogs/containers/introducing-launch-template-and-custom-ami-support-in-amazon-eks-managed-node-groups/) or custom AMI.

Another option is to **use Kubernetes network policies to block access to the Instance Metadata service**. Unfortunately, the [Kubernetes network plugin](https://github.com/aws/amazon-vpc-cni-k8s) that EKS uses does not natively support network policies. The recommended workaround is to add the Calico Network Policy provider, which will be able to pick up your network policy objects and apply them. Note that this is not a full new [CNI plugin](https://kubernetes.io/docs/concepts/cluster-administration/networking/) for your cluster, just a “network policy enforcer” ([Calico supports both modes.](https://www.projectcalico.org/calico-network-policy-comes-to-kubernetes/))

We will be installing NetworkPolicies in later labs, so for this mitigation, lets look at a really cool tool that can help us ID the SSRF bug.

[Semgrep](https://semgrep.dev) is an open source Static Analysis tool that allows you to write rules to detect bugs. We will use the out-of-the-box [Nodejs Security Scan Ruleset](https://semgrep.dev/p/nodejsscan) to scan our code for security issues.

Install Semgrep

Windows installation requires WSL, which may add additional complexity. You can use the playground to complete this challenge.

[Getting started | Semgrep](https://semgrep.dev/docs/getting-started/)

```json
semgrep --config "p/nodejsscan" index.js
```

![[Pasted image 20220309224229.png]]

OR

We can use the Semgrep Playground

[Semgrep](https://semgrep.dev/s/thedeadrobots:ssrf_node_detection)

**Fixing this:**

We can do a few things to fix this, we can use IMDSv2 or ensure a specific HTTP header is present, or add NetworkPolicies to restrict `169.254.169.254` ip's. Those are all infrastructure configurations, how do we fix this code? if we need to really render untrusted URL's we should have the user's browser do it. So instead of the server rendering the URL, we should embed it in an `<iframe>` for the browser to render.

!![[Pasted image 20220309224302.png]]

Flag is buried in the code

> 🏁 Challenge 19 Flag - `k8_ctf{D0NTRENDERUnTRusTEDur1$}`

