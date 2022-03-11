# Challenge 15 - K8 Deployment - Rolling Updates

**Challenge Type:** Builder

ok, before we update, lets just do a quick run to see how we can easily expand our pods.

we have 3 pods running, so lets make this 6 and see how fast this happens.

```json
kubectl get pod

```

```json
kubectl get pod
NAME                           READY   STATUS    RESTARTS   AGE
challenge15-56f7765d8b-cv2s4   1/1     Running   0          4m2s
challenge15-56f7765d8b-wclzt   1/1     Running   0          4m2s
challenge15-56f7765d8b-xqlsm   1/1     Running   0          4m2s
```

Let's apply our updated image with more replicas

```json
kubectl apply -f challenge_15/2-deployment_rolling_update_image.yaml
```

We see we have 6 pods, 3 are starting.

```json
kubectl get pods
```

```json
kubectl get pods
NAME                           READY   STATUS              RESTARTS   AGE
challenge15-56f7765d8b-bdjkx   0/1     Terminating         0          4s
challenge15-56f7765d8b-cv2s4   1/1     Running             0          4m32s
challenge15-56f7765d8b-wclzt   1/1     Running             0          4m32s
challenge15-56f7765d8b-xqlsm   1/1     Running             0          4m32s
challenge15-799d9d4cf7-7wdrq   0/1     Pending             0          4s
challenge15-799d9d4cf7-88cbq   0/1     Pending             0          4s
challenge15-799d9d4cf7-hrgdb   0/1     ContainerCreating   0          4s
challenge15-799d9d4cf7-jsvc9   0/1     Pending             0          4s
challenge15-799d9d4cf7-n5nhw   0/1     Pending             0          4s
challenge15-799d9d4cf7-nkrn7   0/1     Pending             0          4s
```

Eventually they will all be updated. 

```json
NAME                           READY   STATUS    RESTARTS   AGE
challenge15-799d9d4cf7-7wdrq   1/1     Running   0          69s
challenge15-799d9d4cf7-88cbq   0/1     Pending   0          69s
challenge15-799d9d4cf7-hrgdb   1/1     Running   0          69s
challenge15-799d9d4cf7-jsvc9   1/1     Running   0          69s
challenge15-799d9d4cf7-n5nhw   1/1     Running   0          69s
challenge15-799d9d4cf7-nkrn7   1/1     Running   0          69s
```

now go refresh the webpage. 

thats the flag! we just did a rolling update to refresh the container images on all our running pods and expanded them to 6
![rolling](/screenshots/Screen%20Shot%202022-03-09%20at%2010.10.07%20PM.png)

> 🏁 Challenge 15 Flag - `k8_ctf{E@5y8reEzYuPD@T1NG}`



cleanup

```json
kubectl delete -f challenge_15/2-deployment_rolling_update_image.yaml
```