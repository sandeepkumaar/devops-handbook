## Pods
Everything in kubernetes is a **Pod**. It is the smallest deployable unit that is created and managed by k8s.
- Pods can contain more than on container. containers in a pod share the same resources from the pod
- Usually pods are created through workload resources like **deployments, statefulSet, Jobs**
- Containers in the pod can access the *shared storage* through **volumes**
- Each Pod is assigned a unique IP address in the cluster
    - Containers within a pod can communicate with one another using `localhost`
    - Containers in different pods have distinct IP addresses and cannot  communicate by OS-level IPC


make sure minikube is running & docker logged in pull images
```
$ kubectl apply -f simple-pod.yaml

$ kubectl get pods
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          81s   10.244.0.5   minikube   <none>           <none>

$ kubectl describe pod <metadata.name>
```

- NAME: <metadata.name>
- READY: <containers ready/total containers>
- STATUS: Pending | Running | Succeeded | Failed | Unknown   
- RESTARTS   
- AGE   
- IP: <pod unique ip>       
- NODE       
- NOMINATED NODE   
- READINESS GATES
## Namespace
Namespaces are different groups or environments for organisation purposes. more like dbs in an instance
Here pods can communicate with different namespaces.
namespaces are mentioned in `metadata`

```
$ kubectl get pods -A 

NAMESPACE     NAME                               READY   STATUS    RESTARTS      AGE
default       nginx                              1/1     Running   0             9m
kube-system   coredns-7db6d8ff4d-jl6xk           1/1     Running   0             78m
kube-system   etcd-minikube                      1/1     Running   0             78m
kube-system   kube-apiserver-minikube            1/1     Running   0             78m
kube-system   kube-controller-manager-minikube   1/1     Running   0             78m
kube-system   kube-proxy-992d7                   1/1     Running   0             78m
kube-system   kube-scheduler-minikube            1/1     Running   0             78m
kube-system   storage-provisioner                1/1     Running   1 (78m ago)   78m
```
