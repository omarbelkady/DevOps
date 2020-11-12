# DevOps

## Docker
### Images
```
An image is a template for creating an environment of your choice. This environment can be a DB, Web App
or an App that does some time of processing.

An image is also a snapshot a version at any specific moment in time.
Imagine I deploy an image to production and my application was not error free. I can always
roll back to the previous image.
An image contains all the necessities for my application to run: OS, Software and Application Code.

The first layer in the stack is called the base image it can be: a basic OS(e.g. Ubuntu, Debian, Fedora), prepackaged sdk
or application(e.g. Python or Nginx) or a custom image I created.

On top of the base image docker can stack any number of additional layers that modifying the base image
by adding/changing or removing files and dirs

It is worth remembering that every layer contains a large difference between its preceding layer in the
stack.
```

### DOCKER: How To Create A Docker Image 
```
Creating a dockerfile is similar to a text file
RUN is the command used to run shell commands during the container build
for installing packages or compile code or run tests

LAST CMD to specify a command docker will run automatically after starting the container
```

```
FROM debian:buster
COPY . /myproject
RUN pip install flask
CMD ["python", "app.py"]

```

### DOCKER: I create the docker file give it a name of Dockerfile
```
I tell docker to download the image from docker hub 
I then want to copy the project directory into the container
To install python packages from the requirements.txt file
To run shell commands during the container build I use the
RUN directive and follow it with the shell commandd I want
to execute
Next I tell docker how to run the flask app once the container
is launched using CMD and within square brackets and between
quotes I supply it with the shell command
ONLY ONE CMD WILL GET READ BY DOCKER IF YOU SUPPLY PLENTY
only the last one will take effect
NOW SINCE workdir is specified I replace myproject with a dot
```

```
FROM python:3.9
COPY . /myproject
RUN pip install -r /myproject/requirements.txt
CMD ["python", "/myproject/nelanisthebiggestpintosfanb.py"]

```

```
FROM python:3.9
WORKDIR /myproject
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "nelanisthebiggestpintosfanb.py"]
```


### DOCKER: To build the docker file
```
docker build --file Dockerfile
```

#### DOCKER: How to create an image from the docker file make sure you are within your docker project because it will search
#### for the dockerfile
```
docker build .
```


### DOCKER: Container
```
A container is a running instance of an image
To run an nginx container:
```

```docker
docker pull nginx
```

### DOCKER: WORKDIR
```
sets the current working directory in the container
```

### DOCKER: Check if there is any running container
```docker
docker ps
```


### DOCKER: To see the list of running images
```docker
docker images
```

### DOCKER: How To Spin a new container e.g. in this case I am spinning a redis container
```docker
docker run redis
```

### DOCKER: run the latest container from an image
```docker
docker run imagename:tag
docker run nginx:latest
```

### DOCKER: Query The list of Running Containers
```docker
docker container ls
```

### DOCKER: Stop A Running Container Container id is a numerical value
```docker
docker stop containerid
```

### DOCKER: Map Ports From Local Machine to Container here mapping port 256837 on my local machine to port 80 on the container
```docker
docker run -d 256837:80 nginx:latest
```

### DOCKER How To Map Multiple Ports -d means in detached mode
```docker
docker run -d -p 3000:80 -p 8080:80 nginx:latest
```

### DOCKER: How To Restart A Container
```docker
docker stop 252656837c263727
docker start 252656837c263727
```

### Say you finished your day and the next day you do not recall which container you ran
### because docker ps doesn't show you the full list well -a shows you the running and stopped 
### container
```docker
docker ps -a
```

### DOCKER: Sometimes some users phone do not support the latest version which is 5.0 and you want to keep version 4.0 so you can run the image 4.0 and 5.0 the bottom command pulls and start container
```docker
docker run redis:4.0
```


### DOCKER: Ports 
```
I must create a binding port between my local machine and the container.
So say my container is listening on port 3000 I must bind the listening
port 3000 on my machine to the container port 3000

IF YOU OPEN TWO OF THE SAME PORTS(ON YOUR MACHINE) AND MAP THEM TO DIFFERENT PORTS ON THE CONTAINER
YOU WILL FACE A PROBLEM
HOWEVER IF YOU TWO DIFFERENT PORTS ON YOUR MACHINE TO THE SAME PORT IN THE CONTAINER THAT IS OKAY
```


### DOCKER: DEBUGGING CONTAINERS
```docker
docker logs
```

```docker
docker exec -it
```

## Kubernetes
- Open Source container Orchestration Tool
- Helps manage containerized applications in different deployment environments(phy, vir, hybrid)
- Heavily used in small independent applications e.g. Microservices
- No downtime, scalability(high response), disaster recovery

## K8s Components
- Volumes
- Secrets
- ConfigMap
- Deployment
- Ingress
- Pod
- Service
- StatefulSet

```
- Node: simple server, vm
- Pod: Smallest Unit in K8s it is an abstraction over a container
- Usually I reserve 1 pod per application
- Each Pod Gets Its own IP Address
```

### Pods
```
Communicate with each other using a service

Example Say my-app pod uses a service called mongo-db-service to communicate with the DB
Layer of abstraction on top of a container. Dep
```


### Service
```
a static ip address/permanent ip address that can be attached to each pod.
a pod will have its own service and the database will have its own service

Lifecycle of the pod and the service are not connected. If the pod dies, the 
service and the ip address will stay

Communication between Nodes is done through Services
```

### Deployments
```
blueprints for the application pod and allow us to scale up and scale down 
a number of replicas we need. A layer of abstraction on top of pods.

In practice I use Deployments and not pods
```

### External service
```
for your application to run in the browser I must create an external service.
External service is a service that opens the communication from external sources.
Db should not be external because it would be accessible by the public.
Therefore, I create an internal service.

```

### Stateful Set
```
This is used for Stateful apps. Stateful set is meant for applications 
like databases(mysql, mongodb, elastic search). I should use Stateful
Set to create these types of applications and not deployments. Stateful
set will know how to take care of pods and scaling them up. Deploying
DB applications using stateful sets in K8s cluster can be tedious at times.
It is definitely harder to work with than Deployments. I must always
remember to host db applications outside the K8s cluster and have just the 
deployments/stateless applications which replicate and scale without 
any error raised inside the k8s cluster. Now that both my nodes are load
balanced, it is clearly easy to notice that my setup is more robust and if
node 1 server was rebooted/crashed and nothing can run on it, I would always 
have a 2nd node aka node 2 server with application and database running on
it. The application in the 2nd node would still be accessible by the user until
the two replicas get recreated this prevents downtime.

```
    
### Ip Address Ingress
```
I want my application to have a secure protocol and a domain name
NOT my Ip which is why I use Ingress. This is used to route
traffic to the cluster.
```

### External Configuration Type1: ConfigMap
```
ConfigMap is your external configuration to your application.
ConfigMap contains configuration data such as: URLs of DB, other services
How To Use ConfigMap in K8s: Connect it to the Pod so that the pod gets the data that ConfigMap contains
```

### External Configuration Type2: Secret
```
Similar to ConfigMap except that it is used to store secret data: credentials
and store not in plain text format but in base64 encoded. 
Things that go in the Secret e.g. certificates, passswords, etc.

How to make secret talk talk to pod: connect it for it to know who to authenticate
```


### Volumes
```
Attaches a physical Storage on a harddrive to the pod. This storage can be on a 
local machine, remote storage(outside of the k8s cluster). This is used for data
persistence
```


### Safety
```
Imaging everything is running perfectly and a user can access the application through the browser. 
Imagine the application pod crashes/dies, because there was a new container image built.
This would trigger downtime which would prevent a user from using the application.
The beauty is that k8s is always replicating everything on multiple server. There would always
be a parallel node running which would be a clone of the application 
```

### Kubernetes Architecture
```
main components of K8s architecture are worker nodes/servers and each node
will have multiple application pods with containers running on that specific
node.
Every node MUST have preinstalled three processes that used to manage and schedule the pods:

Nodes are the cluster servers that actually do the work aka worker nodes

```
- Process #1: Container Runtime
- Process #2: Kubelet this interacts with both the container and node
Kubelet start the pod with a container inside
- Process #3: Kube Proxy(must be installed on every K8s worker node) is used to forward requests and must be installed on every node


### How To Interract with a cluster
```
Thanks to Master Nodes is how Interract with a cluster.
A Master Node Controls the Cluster State and the worker nodes
```

### Master node processes or service that run in this chronological order
1- API Server: when I(user) want to deploy a new app in k8s cluster I interract with the api server through a client(UI, CLI, API). API Server is aka a the cluster gateway. Also it
is a gatekeeper for authentication 

2- Scheduler: send an API server a request to schedule a new pod. The API Server 
validates my request first then hands it over to the scheduler in order to start the application pod on one of the worker nodes.Scheduler has a intelligent feature built into it to decide on which specific worker node the next pod will be scheduled. Scheduler just decides on which node the new pod will be scheduled

3- Controller Manager: Detects State Changes(e.g. Pods Crashing) and tries to recover the 
cluster state. It makes a request to the Scheduler to reschedule the deadd pods and the 
scheduler decides based on the resource calculation which worker nodes whould restart
the pods again and makes a request to the corresponding kubelet.

4- etcd: This is a key value store of a cluster state or is the cluster brain. Any changes
happening in the cluster gets saved in this key value store. The application data is not 
stored in the etcd. Etcd holds the current status of any K8s component.

### Sample Cluster Setup
- 2 Master Nodes(use less resources e.g. CPU, RAM and Storage)
- 3 Worker Nodes(use up more resources)

### How To Add A New Master/Node Server to your application
- get a new bare server
- install all the master/node processes
- add it to the cluster

### Namespace
- can organize resources in namespaces
- can have multiple namespaces within a cluster
- Things that are deployed in a namespace are: system processes, 
- I can think of a namespace as a nested cluster. This means a virtual cluster within a cluster
- When you create a cluster K8s gives you 4 Namespaces by default
    kubernetes-dashboard: shipped automatically with minikube this doesn't exist std cluster
    - kube-system: we are not supposed to create or modify anything in kube-system 
    - kube-public: has publicely accessible data and has a configMap which contains cluster info
    - kube-node-lease: keeps a record of the hearbeats of nodes 
        - every node here in node-lease gets its own object in the namespace
    - default: used for you to create resources at the beginning
#### Usecases Of A Namespace
- Imagine having a default namespace which is provided by K8s and I create all my resources there in addition to the default namespaces that were shipped by K8s 
- If I have a complex application which has multiple deployments which create replicas of many pods and I have resources such as services and ConfigMap. Sooner than later my default namespace will be filled with different components
- The proper way to use a namespace is to group namespace by their appropriate resource
    - database namespace: I deploy my database and its required resources here
    - Monitoring Namespace: This deploys Prometheus and all its required resources
    - Elastic Stack Namespace: This deploys Elastic Search and Kibana resources 
    - Nginx-Ingress: This deploys Nginx-Ingress Resources
- Do not use a namespace if you are working on a small project and up to 10 users.
- Working With Teams in one application
- A cluster which I want to host both staging and development environment. This is done
because say I use nginx controller/elastic stack for logging, this will enable me 
to deploy it in one cluster and use for both environments. This will save me the hassle
of deployment both environment to two indepedent clusters. Staging now use both resources
as well as the development environment
- Blue/Green Development: Blue Production's version namespace is different than Green Production's
version namespace. Blue's Production is active and Green is the subsequent Production version.
Before deployment when they are both in Production they both use the same resources this prevents
the need to create a separate cluster   
#### Error you can reach if you are on Team 1 and there is Team 2 also working on an app
```
if your team(Team 1) and the other team(Team 2) are using the same cluster. Team1 deploys
the app which is called cs375fb-deployment and this team(team1) has a certain configuration.
If the other team(Team2) had their deployment named cs375fb-deployment too which is the same name 
of team1's deployment name even though Team 2 has a different configuration than
Team 1. If Team 2 deployed their configuration to the cluster they would overwrite Team 1's 
deployment.

Even if Team 2 had Jenkins to automate their tasks they wouldn't know they disrupted another
team's deployment. To avoid such conflicts I use namespaces so that each team can work on their
own namespace without disrupting another team's progress.

```

#### How To Create A Namespace 1st Way
```bash
kubectl create namespace nameOfYourNamespaceYouWishToCreate
```


#### How To Create A Namespace 2nd Way Using A Config File(Best And Recommended)
```bash
kubectl create namespace nameOfYourNamespaceYouWishToCreate
```

#### Display the List Of Namespace In the Cluster
```bash
kubectl get namespace 
```


### Minikube and Kubectl
#### Minikube
```
In K8s world, when I am ready to setup a K8s cluster in production I would have at least 2 
master nodes and multiple worker nodes. Instead of running my entire application which will
require an enormous amount of memory and needs a lot of resources allocated to it. I use a
minikube. Minikube is a one node K8s cluster where the master processes and the node processes 
both run on the same machine aka the same node.
```

#### Kubectl
```
One of the master processes called API Server is actually the main entry point into the K8s cluster.

To Talk to the api server is through a client e.g. UI, K8s API or a CL Tool(Kubectl)

Kubectl can control a MiniKube cluster, cloud cluster or hybrid cluster
```

### Kubectl commands


#### How to create a component
```bash
kubectl create [whatyouwanttocreategoeshere]
```

#### How to create a deployment
```bash
kubectl create deployment NAME --image=image [--dry-run] [options]
```

#### How To Create A Nginx deployment
```bash
kubectl create deployment nginx-depl --image=nginx
```

#### How To Modify a deployment
```bash
kubectl edit deployment [nameofdeployment] 
```

#### How To Delete A Deployment
```bash
kubectl delete deployment [nameOfDeployment]
```

#### Configuration File
Every Configuration File Has Three Parts to It and syntax for yaml files is strict indentation
- metadata: contains the labels
- specification aka spec have attributes specific to the kind of: contains the selectors 
- status-Automatically generated and started by K8s Desired state and actual state must equal or K8s will now that it must fix it

#### Sample Configuration File -- nelanlovescstsfnginx-deployment.yaml
```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
    name: nginx-deployment
    labels: 
spec:
    replicas: 2
    selector: 
        matchLabels:
            app: nginx
    template:
        metadata: 
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx
              image: nginx:1.16
              ports:
              - protocol: TCP
                port:
                - containerPort: 8080

```
```
The service is accessible at a different port than deployment. This is because each one
must send a separate request. Say the DB Service sends a request to the Nginx Service
it will need to send it on say port 80. The service must also know to which pod it should
forward the request because different pods listen on a unique port within the container
this is the targetPort key-value pair. 

```

#### How To Check that the service in the pod has been assigned to the right port
```bash
kubectl describe service nginx-service
```

#### How To Get more information about a pod's endpoint and ip address forwarded to
```bash
kubectl get pod -o wide
```


#### Sample Configuration File -- nelanlovescstsfnginx-service.yaml
```yaml
apiVersion: apps/v1
kind: Service
metadata: 
    name: nginx-service
spec:
    selector:
        app: nginx 
    ports:
        protocol: TCP
        port: 80
        targetPort: 8080
```

### How to connect Services To Deployments
```
Within the yaml file within the metadata section there is labels and a key of app and a value of nginx
Within the service yaml file within spec section there is selector and a key of app and a value of nginx

Service must know which pods are registred with it. The connection is made through the selector of the label
```
### How to connect deployment to pods
```
pods get the label through the template blueprint. The label in metadata is matched
with the selector in the specification. This is done so that we know which pod belongs
to it.
```
### Blueprint for Pods
- A Template is a blueprint for a pod

#### How To Apply A Configuration File -f is file cstsf=cs375 K8s will now whether to create or update    
```bash
kubectl apply -f config-cstsffb-file.yaml
```

#### Check if we have any pods running
```bash
kubectl get pod
```
### Useful Debuging Commands In K8s
#### Debug A Pod
```bash
kubectl logs [podname] 
```

#### Get Additional Information on a Pod useful when a pod is not starting
```bash
kubectl describe pod [podname]
```

#### Exec- gets the interactive terminal of the application container using the it abbreviation
```bash
kubectl exec -it [podname] -- bin/bash
```

#### Check if there are any services running
```
kubectl get services
```

#### Replicaset: Manages the replicas of a pood
```
kubectl get replicaset
```

#### Get the status of different kubernetes components of your choice(node, or pod or replicaset)
```
kubectl get nodes|pod|services|replicaset|deployment
```

#### Layers of Abstraction in K8s
```
Deployment manages a ReplicaSet
ReplicaSet manages all the replicas of that Pod
Pod is an abstraction of a container
Everything below Deployment is managed by Kubernetes
```
