# Working with minikube k8s
## in minikube master node & worker nodes are present on same server & share same clester

## Frstly install minikube , refere installation file.

# minikube k8s commands:

minikube ssh                   # to enter inside minikube cluser & get access of pod

exit                           # to logout minikube

## we can communicate with cluster using GUI/CLI 
## we can not run commands inside cluster

 kubectl version              # check version
 kubectl cluster-info         #to get cluster info & check kubectl cummunicat with cluster or not 

 ## command to create pod
 syntax : kubectl run <pod_name> --image <image_name>

 Ex. 
 kubectl run first-pod --image nginx                        # here i created first pod using nginx image

kubectl get pods              #list out all pods

kubectl get nodes             #listout all nodes , here as working with minikube so only one node is present

## for detail information of nodes & pods
kubectl get nodes -o wide         
kubectl get podes -o wide

## kubernetes objects: refer notes for details information

kubectl get <object_name>   -------listout object

## Working with objects

kubectl get namespace                            # listout all namespace, 
Oonce run this command , get 4 name spaces alreay presents,

syntax: kubectl get pods --namespace <namespace_name>  #to check pods of perticular namespace.

1. defalut: if we can not mention any name same then pod create in default name space by default.
kubectl get pods --namespace default    # least default pods
2.kube-node-lease: This namespace is used by the Kubernetes Node Lease feature, which helps nodes inform the control plane about their presence and health status. It contains Lease objects created by each node in the cluster.

kubectl get pods --namespace kube-node-lease  # list pods in kube-node-lease

kube-public: This created automatically and is accessible to all users.
 It's primarily used for storing resources that should be accessible to all users, such as cluster-wide configuration or informational data.

kubectl get pods --namespace kube-public     # list kube-public pod

kube-system: This namespace contains Kubernetes system resources and components that are essential for the functioning of the cluster.
 It includes components like kube-proxy, kube-dns, kube-scheduler, kube-controller-manager, and others.

 kubectl get pods --namespace kube-system     # list kube-system pods

## create own namespace

syntax:kubectl create namespace <namespace name>    # create namespace
Ex. kubectl create namespace my-ns

kubectl get pods --namespace my-ns    # list pod in namespace

create pod in perticular namespace
syntax: kubectl run <pod name> --image <image name> --namespace <ns name>
ex.
kubectl run ns-pod1 --image httpd --namespace my-ns

kubectl get pods    # list default ns pods
kubectl get pods -n my-ns    #list pods in pertiular namespace

## command to list all objects /resources
kubectl api-resources   # listout all objects in k8s

NAMESPACED =true : pod created in namespace
NAMESPACE =false : pod created anywhre in claster

KIND: its like liberary where all data stored

## as we know we have single node cluster /minikube cluster,
## we can not access pod from outside the cluster(from host) but access from inside the cluster so to enter in cluster need to take ssh of cluster(minikube). 

kubectl get pods
kubectl get pods -o wide
curl <pod_ip>      # trying to access pod, but it stuck as we are  access from host ie from outside the cluster, need to enter in cluster 

minikube ssh         # enter to cluster
curl <pod_ip>         # successfully access application

Node: kubctl can not work inside the cluster

## if wanted to access same pod from outside cluster then we use expose service (similar like port mapping in docker)

syntax: kubctl expose pod <pod_name> --type <service_type> --port 80 -n <namespace_name>