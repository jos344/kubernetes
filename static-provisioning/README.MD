## A little test of static persistent storage using hostPath

Create a persistent volume.
```
❯ kubectl create -f my-pv.yaml
persistentvolume/example-volume created
```
Check the resource.
```
❯ kubectl get pv
NAME             CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
example-volume   1Gi        RWO            Retain           Available           hostpath                8s
```

Create a persistent volume claim.
```
❯ kubectl create -f my-pvc.yaml
persistentvolumeclaim/example-claim created
```

Check that it’s bound to the persistent volume.
```
❯ kubectl get pvc
NAME            STATUS   VOLUME           CAPACITY   ACCESS MODES   STORAGECLASS   AGE
example-claim   Bound    example-volume   1Gi        RWO            hostpath       3s
```

Create a busybox pod with reference to the persistent volume claim.
```
❯ kubectl create -f busybox.yaml
pod/busybox created
```
Check the resource.
```
❯ kubectl get pod
NAME      READY   STATUS    RESTARTS   AGE
busybox   1/1     Running   0          6s
```

Create a file in the container.
```
❯ kubectl exec busybox -- touch /host/app.py
```

Delete and recreate the pod.
```
❯ kubectl delete pod busybox
pod "busybox" deleted
❯ kubectl create -f busybox.yaml
pod/busybox created
```

Verify that the file has been persisted.
```
❯ kubectl exec busybox -- ls /host/
app.py
```
