<p align="center">
<img src="screenshots/k8ctf_logo.png" alt="Devslop_K8_ctf_logo" width="300"/>
</p>

This workshop/CTF is part of the [OWASP DevSlop](https://www.devslop.co) project was hosted at the 2021 Diana Initiative CTF.  It is intended to be beginner-friendly CTF on a lightweight, low-cost Kubernetes setup, that you can run in your own AWS account. Its format is a little different than normal CTF's. The goal is to introduce you gently to Kubernetes concepts (learning exercises) and then to break the security of something (breaker exercises) and finally to fix the thing you just broke (builder exercises).  We hope that this format helps you learn the introductory concepts well enough to understand why you are able to violate security and then solve the underlying security problem.

Big shout out to the Kops team. Without Kops this would have made individualized Kubnernetes clusters near impossible to be both low cost and light weight.

Start by going to the [Start_Here.md](https://github.com/DevSlop/k8ctf/blob/master/Start_here.md)

| Challenge     | Description   | Type  |
| ---------     | -----------   | ----  |
| Challenge 01 | K8 Overview - History of Virutalization | Learner |
| Challenge 02 | K8 Overview - Overview of Docker/Containers | Learner |
| Challenge 03 | K8 Overview - What is Orchestration? | Learner |
| Challenge 04 | K8 Overview - History of Kubernetes | Learner |
| Challenge 05 | K8 Overview - Kubernetes Architecture | Learner |
| Challenge 06 | K8 Overview - Kubernetes Objects & Organization | Learner |
| Challenge 07 | K8 Setup - Logging into Kubernetes | Builder |
| Challenge 08 | K8 Setup - Using `kubectl` | Builder |
| Challenge 09 | K8 Setup - Kubernetes AuthN/AuthZ Overview | Learner |
| Challenge 10 | K8 Setup - Kubernetes Dashboard | Builder |
| Challenge 11 | K8 Networking - ClusterIP | Builder |
| Challenge 12 | K8 Networking - NodePort | Builder |
| Challenge 13 | K8 Networking - LoadBalancer | Builder |
| Challenge 14 | K8 Deployment - Simple Deployment | Builder |
| Challenge 15 | K8 Deployment - Rolling Updates | Builder |
| Challenge 16 | K8 Security - Finding Creds in Code | Breaker |
| Challenge 17 | K8 Security - Using Kube Secrets | Builder |
| Challenge 18 | K8 Security - SSRF on K8 | Breaker |
| Challenge 19 | K8 Security - Finding SSRF bugs with Semgrep | Builder |
| Challenge 20 | K8 Security - Container Escape to Host | Breaker |
| Challenge 21 | K8 Security - Intro to OpenPolicyAgent | Learner |
| Challenge 22 | K8 Security - OPA to Restrict Host Access | Builder |
| Challenge 23 | K8 Security - Namespace Bypass | Breaker |
| Challenge 24 | K8 Security - Prevent Namespace Bypass w/ Cilium | Builder |
| Challenge 25 | K8 Security - Consume Cluster Resources / DoS | Breaker |
| Challenge 26 | K8 Security - OPA to Enforce Resource Limits | Builder |
| Challenge 27 | K8 Security - Scanning Containers for Vulnerabilities | Breaker |
| Challenge 28 | K8 Security - OPA to Enforce Trusted Image Sources | Builder |
| Challenge 29 | K8 Security - Using Kube-Bench | Builder |


#TODO
Support / Maintenance