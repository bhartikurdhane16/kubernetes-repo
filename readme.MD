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

