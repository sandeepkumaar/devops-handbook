## Containers
Containers are isolated environments on the machine which share the same OS(kernel) but separate filesystem, share of CPU, Memory.
Containers are bundles that can be deployed to any clouds, onprem and OS distributions making it suitable for **distributed systems** 

## Kubernetes 
Kubernetes is a framework/tool to manage containers in a distributed environment - **kubernetes cluster**  built on mulitiple machines (onprem, cloud etc)
Kubernetes cluster provides
- Service Discovery & Load balancing
- Storage orchestration
- Automated Rollouts and Rollbacks
- Automatic resource allocation
- Self healing
- Secret and Configuration management
- Batch Execution
- Horizontal scaling
- IPv4/v6 
- Extensibility


## Kubernetes Components & Architecture & Containers(runtime)

## Workloads
Different types of Applications that run on kubernetes. Built-in kubernetes workloads
- Deployment & ReplicaSet
- StatefulSet
- DaemonSet
- Job & CronJob



## misc
shell completions
```
$ source <(kubectl completion zsh)
```

## Cheatsheet
```
$ kubectl get namespaces
$ kubectl get nodes

$ kubectl get pods
$ kubectl get pods -o wide
$ kubectl delete pod <pod.name>
$ kubectl describe pod <pod.name>

$ kubectl get deployments
$ kubectl get services

```

