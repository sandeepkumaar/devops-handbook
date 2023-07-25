# Kubernetes 

## Course notes from KodeKloud

## k8s definitions

- What is Kubernetes
    - By Google
    - An open-source system for automating deployment, scaling, and management of containerized applications.
    - Alternatives --> Docker Swarm, Mesos, rkt
- Node
    - A physical or virtual machine where k8s is installed
    - Place where containers are launched by kubernetes
- Cluster
    - Group of Nodes
    - 1 Master and 'n' nodes
- Master
    - Responsible to manage and monitor the cluster. 
- Components of K8s
    - API server (master)
    - etcd (master)
    - kubelet (worker node)
    - Container runtime (worker)
    - Controller (master)
    - Scheduler (master)
- kubectl
    - Command line tool for kubernetes
- CRI
    - Container Runtime Interface
- OCI
    - Open Container Initiative

- Namespace
    - In-built : 
        - kube-system
        - Default
        - kube-public
    Provides Isolation to various resources/objects

- Pods
    - A single instance of an application
    - The smallest object that you can create in k8s
    - Docker Container is encapsulated in a Pod
    - 1 : 1 relationship with containers
- Multi Container Pods
    - Deploying multiple containers in the same Pod
    - Not advisable for scaling but can be a benefit if a helper container needs to be deployed along side another container.

- Replication Controllers (old) | Replica Set (new)
    - Created Multiple Instances of Pods
    - HA
    - Maintains the specified no.of Pods at any point of time even if pods were added or deleted manually. 
    - Load Balancing and Scaling
        - Works across multiple nodes across clusters
    - Replica Set has `selectors` which should match the label of any pods created
        - This makes it easier to manage pods that are already created using labels
    - Replica set needs the template of pod so that it can create the pod when needed.

- Deployment
    - Deploy multiple docker container instances
    - Upgrade docker instances one by one
    - Rollout and Rollback
    - Pause and Resume updates to environments
    - container << pod << replicaset << deployment
    - During upgrade, a new replicaset is created and new containers are created one by one by deleting containers in the old replicaset

- Networking
    - Private network will be created for every node
    - Every Pod will be assigned an internal IP address
    - Default range 10.244.0.0
- Cluster Networking
    - Kubernetes Cluster does not handle networking automatically when we have multiple nodes.
    - Rules : K8 expects this setting from the user 
        - All containers/PODs can communicate to one another without NAT
        - All nodes can communicate with all containers and vice-versa without NAT

- Services
    - Connect applications together
    - Enables loose coupling in micro-services architecture
    - Types : 
        - NodePort - External communication
            - Port Range 30000 to 32767 
        - ClusterIP - Internal communication
        - LoadBalancer - External communication

- Resource Quota
    - Allocate defined resources to namespaces

- ConfigMap
    - Passing environment variables
    - Instead of `spec`, config-map will have `data`

- ServiceAccount
    - Used to access REST APIs of Master Node
    - For example, jenkins would have a service account to access APIs for automation
    - Pods inside K8 would have default service accounts for accessing the APIs. 

- Taint and Toleration
    - Enables to schedule which POD should go to which node.
    - Bug, Person and repellant example
    - kubectl taint nodes `node-name` `key=value`:`taint-effect`
    - Taint Effects
        - NoSchedule
        - PreferNoSchedule
        - NoExecute
    - Restricts node to accept only certain toleration
    - Doesn't guarantee that the Pod will get scheduled only to a particular node
    - To untaint, add a minus at the end
        - kubectl taint nodes `node-name` `taint-effect`-

``` 
tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal
```

- Node Selectors
    - Enables to schedule the Pod to a node based on resource availability
    - Pod demanding large node, should have the nodeSelector as large and the node must also be labelled as large
    - kubectl label nodes `node-name` size=Large
    - Doesn't support range of selectors

- Node Affinity
    - Enables us to select nodes in a range based on conditions
    - Affinity Types
        - requiredDuringSchedulingIgnoredDuringExecution
            - Existing pods will not be removed from the node 
        - preferableDuringSchedulingIgnoredDuringExecution
            - Prefers to schedule the pod with the node label. Else schedule it elsewhere ke
        - requiredDuringSchedulingRequiredDuringExecution
            - Existing pods will be removed from the node

```
affinity:
    nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
                - key: size
                  operator: In
                  values:
                  - large
                    medium
                      
```

|        | During Scheduling | During Execution |
| ------ | ----------------- | ---------------- |
| Type 1 | Required          |    Ignored       |
| Type 2 | Preferred         |    Required      |
| Type 3 | Required          |    Required      |


- Multi-Container Pods
    - Design Patterns
        - Side Car --> Logging Agent
        - Ambassador --> Proxy
        - Adapter --> Transformation


### kubectl commands

- kubectl get all --namespace=`custom-namespace`
- kubectl get nodes
- kubectl get nodes -o wide
- kubectl version
- kubectl run `[pod-name]` --image `[image-name]`
- kubectl delete pod `[pod-name]`
- kubectl get pods
- kubectl describe pod `[pod-name]`
- kubectl get pods -o wide

- kubectl create -f pod-definition.yml --record
- kubectl apply -f pod-definition.yml
- kubectl replace -f pod-definition.yml
- kubectl scale --replicas=6 -f replica.yml
- kubectl scale replicaset --replicas=6 `[replicaset-name]`

- kubectl run `[pod-name]` --image=`[image-name]` --dry-run=client -o yaml > `[out.yaml]`
    - This command will output the yaml definition equivalent to the command.

- kubectl create -f replication-controller.yaml
- kubectl get replicationcontroller

- kubectl rollout status `[deployment-name]`
- kubectl rollout history `[deployment-name]`
- kubectl rollout undo `[deployment-name]`

- kubectl config set-context $(kubectl config current-context) --namespace=dev
- kubectl get pods --all-namespaces

- kubectl create configmap \
    `config-name` --from-literal=APP_COLOR=blue --from-file=file.properties


## Yaml Structure

- apiVersion - case sensitive
    - Pod : v1
    - Service : v1
    - ReplicaSet : apps/v1
    - Deployment : apps/v1
- kind
    - Pod | Service | Replicaset | Deployment
- metadata
    - name : `[pod-name]`
    - labels : 
        - app : `[app-name]`
        - can have any Key Value pairs. Would be easier to group pods based on the labels
- spec
    - containers --> Array
        - name : `[container-name]`
          image : `[image-name]`


## Questions

- Does Replica Set replace Pod definition files?
    - Since RS needs the pod definition under template section, do we need a separate pod definition file?
- 