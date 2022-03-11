## Challenge 8


**Challenge Type:** Builder

1.  Get Version - `kubectl version --short`
    
2.  List Namespaces
    
    ```
    kubectl get ns
    kubectl get nodes
    kubectl get services# List all services in the namespace
    kubectl get pods --all-namespaces# List all pods in all namespaces
    kubectl get pods -o wide# List all pods in the current namespace, with more details
    kubectl get deployment my-dep# List a particular deployment
    kubectl get pods# List all pods in the namespace
    kubectl get pod my-pod -o yaml# Get a pod's YAML
    ```
    
3.  Explore a YAML file, just to get more familiarized with yaml and how kubernetes uses it.
    
    [](https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml)[https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml](https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml)
    
    To get the flag, what is the version being shown when you do `kubectl get nodes`
    
> 🏁 `k8_ctf{v1.20.7}`
    
