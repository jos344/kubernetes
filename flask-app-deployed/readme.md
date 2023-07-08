## Python flask app running on minikube cluster. 

### Deployment creates 5 pods each running container built from custom image.

Create the service.

```
❯ kubectl create -f service.yaml
service/hello-world-service created
```

Create the deployment.
```
❯ kubectl create -f deployment.yaml
deployment.apps/hello-world-deployment created
```

Check that all objects are up.
```
❯ kubectl get all
NAME                                          READY   STATUS    RESTARTS   AGE
pod/hello-world-deployment-7764c5d85b-9qng6   1/1     Running   0          35s
pod/hello-world-deployment-7764c5d85b-dw4ms   1/1     Running   0          35s
pod/hello-world-deployment-7764c5d85b-mrzjr   1/1     Running   0          35s
pod/hello-world-deployment-7764c5d85b-n2hvx   1/1     Running   0          35s
pod/hello-world-deployment-7764c5d85b-zffpx   1/1     Running   0          35s

NAME                          TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)          AGE
service/hello-world-service   LoadBalancer   10.97.185.247   192.168.49.2   8080:30001/TCP   80s
service/kubernetes            ClusterIP      10.96.0.1       <none>         443/TCP          19h

NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-world-deployment   5/5     5            5           35s

NAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/hello-world-deployment-7764c5d85b   5         5         5       35s
```


Check that app is running.
```
❯ curl 192.168.49.2:30001
Hello world from Python Flask
Greetings from container: hello-world-deployment-7764c5d85b-vs4dj
```
