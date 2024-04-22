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
 delete pod <pod_name>                                      # delete pod

kubectl get pods              #list out all pods

kubectl get nodes             #listout all nodes , here as working with minikube so only one node is present


## for detail information of nodes & pods
kubectl get nodes -o wide         
kubectl get podes -o wide


## pod workflow 
kubectl get podes -o wide   #command gives below details
ready=1/1 : only one contaoiner present & running that pod
restart: how many time container restarted
IP: pod ip
Node: shows our pod running on which node
readyness: pods are ready to create or not
Nominate:Indicates if a node has been nominated for the Pod

## create pod with labels
kubectl run pod4 --image nginx --labels app=erp-app,env=prod,project=xyz --port=80   
kubectl get pods --show-labels

## kubernetes objects: refer notes for details information

## there are multiple objects in k8s like, namespace, services, configmap, stateful-set, stateless-set, daemon-set, pod, replication controller,deployment,replica  set etc.

kubectl get <object_name>   -------listout object

ex kubectl get configmap      -------listout congigmap

## Working with objects
syntax: kubectl get pods --namespace <namespace_name>    #command to  check pods of perticular namespace.

kubectl get namespace                            # listout all namespace 

here 4 namespace already preset.

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

syntax:kubectl create namespace <namespace name>                            # create namespace
syntax: kubectl get pods --namespace <namespace name>                       # list pod in specific namespace
syntax: kubectl run <pod name> --image <image name> --namespace <ns name>   # command create pod in perticular namespace
syntax: delete pod <pod_name> -n <namespace-name>                           # delete pod
Ex. 
kubectl create namespace my-ns
kubectl get pods --namespace my-ns    
kubectl run ns-pod1 --image httpd --namespace my-ns
kubectl delete pod pod1 -n my-ns

kubectl get pods                                                            # list default ns pods
kubectl get pods -n my-ns                                                   #list pods in pertiular namespace

## command to list all objects /resources
kubectl api-resources   # listout all objects in k8s
kubectl api-version     #show version

NAMESPACED =true : pod created in namespace
NAMESPACE =false : pod created anywhre in claster

KIND: its like liberary where all data stored

## as we know we have single node cluster /minikube cluster,
## we can not access pod/application from outside the cluster(from host) but access from inside the cluster so to enter in cluster need to take ssh of cluster(minikube). 

kubectl get pods
kubectl get pods -o wide
curl <pod_ip>      # trying to access pod, but it stuck as we are accessing from host ie from outside the cluster, need to enter in cluster 

minikube ssh         # enter to cluster
curl <pod_ip>         # successfully access application

Node: kubctl can not work inside the cluster

## if wanted to access same pod from outside cluster then we use expose service (similar like port mapping in docker)

syntax: kubctl expose pod <pod_name> --type <service_type> --port <port_no> -n <namespace_name>   # map pod port=random service port

kubectl expose pod <pod_name> --type=Service --port=<port_number> --target-port=<target_port> -n <namespce>   # map pod port =with specific port

note: namespace not given then it consider default port

there are 4 Service types use to expose pod port:

1. ClusterIP : is used when you wanted to create an internal service accessible within the cluster.

kubectl expose pod my-pod --type=ClusterIP --port=8080 --target-port=8080

2. ExternalName: is used to provide a DNS alias to an external resource outside of the Kubernetes cluster. This is useful when you want to reference services that exist outside of your Kubernetes cluster, such as databases, APIs, or other services hosted in different environments.

kubectl create service externalname <service_name> --external-name=<external_dns_name>

3. LoadBalancer:use when you wanted to expose service outside the network (access from outside the cluster)

kubectl expose pod my-pod --type=LoadBalancer --port=8080 
kubectl get svc      # shows pending state but LB is in working condition




4. NodePort: use when you wanted to expose service outside the network (access from outside the cluster)

kubectl expose pod <pod-name> --type=NodePort --port=<pod port> --target-port=<target_mapping_port>   # target not given then it map with random port
kubectl get svc -n <namespace_name>    # listout services at pod, namespace not given ithn it will listout for default pod
kubectl get pod <pod_name> -n <namespace_name> -o wide    # detail information of pod , not use namespce give info of default pod
minikube ip                     #command goves minikube ip 
http://<minikube_ip>:<mapped_nodeport>    # used combination of minikube ip & port no to get access of application over external network
OR curl <minikube_ip>:<mapped_nodeport>    #access with combinate of minikube ip & node port
kubectl delete svc <service_name>     #delete service
kubectl run --help    #helping command


Ex.
kubectl expose pod ns-pod1 --type NodePort --port 80 -n my-ns     #it will map my PODPort with= random nodeport & it will create the service
kubectl get service -n my-ns           # here we got service type & mapped port
OR    kubectl get svc -n my-ns         # we can also use short form of service it get when run #cubectl api-resources
minikube ip 
kubectl get pod ns-pod1 -n my-ns -o wide              # here we got pod ip 
http://192.168.49.2:32766     OR curl 192.168.49.2:32766        # try to access app using minikube ip & node port 











