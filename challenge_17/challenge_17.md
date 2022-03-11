### Challenge 17- Fixing Secrets in Code

**Challenge Type:** Builder

We need to fix the issue from challenge 16 and remove the secret in code.

Kubernetes has two vehicles for removing sensitive or configuration related material in deployments:

-   Config Maps
-   Kube Secrets

In this exercise we will use kube secrets to remove the username / password from the deployment and redeploy the app.  

```json
// Deploy the challenge
kubectl apply -f deployment_starwars_secrets.yaml

// get the IP of the service
kubectl get service
```

Browse to the loadbalancer ip and ee see that the database is having an authentication error because our user:password is 'undefined'


![[Screen Shot 2022-03-09 at 10.28.29 PM.png]]


To fix this, we need to create a `starwars_secrets.yaml` file so that our deployment can pull the username / password from kube secrets. We know the user name and password from the previous challenge.

User: devslop

Password: [09CweCYpHc5tOvwS](https://www.base64encode.org/)

```json
apiVersion: v1
kind: Secret
metadata:
  name: mongodbsecret
type: Opaque
data:
  username: ZGV2c2xvcA==
  password: MDlDd2VDWXBIYzV0T3Z3Uw==
```

Remember secrets need to be [base64 encoded](https://www.base64encode.org/).

When you think you have the correct secrets apply it to the cluster

```json
kubectl apply -f starwars_secrets.yaml

// restart the deployment
kubectl rollout restart deployment/challenge17

// wait until all the pods have updated
kubectl get pods
```

Refresh the page and you should see the flag.  be patient its a rollout restart!

> 🏁 Challenge 17 Flag - `{k8_ctf{hiDINmY$3cREts}}`


cleanup

```json
kubectl delete -f deployment_starwars_secrets.yaml
```

<aside> 🚨 DON'T DELETE THE SECRET

</aside>