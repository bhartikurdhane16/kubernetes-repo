## working with minikube k8s
## in minikube master node & worker nodes are present on same server & share same clester

## Frstly install minikube , refere installation file.

## minikube k8s commands.

minikube ssh              # to enter inside minikube & get access of container

exit   # to logout minikube
## commands to communicate with cluster(GUI/CLI)
# we can not run commands inside minikube

 kubectl version        # check version
 kubectl cluster-info       #to get cluster info & check kubectl cummunicat with cluster or not 

 ## create pod
 syntax : kubectl run <pod_name> --image <image_name>

 Ex. 
 kubectl run first-pod --image nginx                        # here i created first pod using nginx image

 kubectl get pods              #list out all pods

kubectl get nodes     #listout all nodes , here as working with minikube so only one node is present

## for detail information of nodes & pods
kubectl get nodes -o wide         
kubectl get podes -o wide

here we can not access application from external network, for that we need to use service like mapping in docker for this expose use in K8S

## kubernetes objects
#kubectl get <object_name>   -------listout object

## working with objects

kubectl get namespace                    # listout all namespace, 
there are 4 name spaces alreay presents,

syntax: kubectl get pods --namespace <name_>

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
