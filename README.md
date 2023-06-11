# K8s

---
## Introduction

This repo holds the manifest files for the K8s (1.22) cluster. 
There are 6 manifest files such that when you apply it on the K8s cluster it will produce below diagram. Note: there is a high-quality version of this figure (K8s-Diagram.pdf)

![](https://i.imgur.com/kwbVMol.jpg)

## Key Features

The 6 YAML files are defined as below

### 1- client-depl

This file has the *deployment* code of the web app along with *service* code.

- The web app is written in typescript using react JS library.
- The docker container/Pod is exposed on port 3000.
- The pod is set for 1 replica.
- The pod request resources are memory: 200Mi and CPU: 100m.
- The service type is *ClusterIP* and exposes port 3000 of the Pod.

### 2- server-depl

This file has the *deployment* code of the backend server for the web app along with *service* code.

- The web app is written in NodeJS.
- The docker container/Pod is exposed on port 3000.
- The pod is set for 1 replica.
- The pod request resources are memory: 200Mi and CPU: 100m.
- The pod limit resources are 200Mi and CPU: 100m.
- The server pod takes the Mongo-URL from Secret.
- The service type is *ClusterIP* and exposes port 3000 of the Pod.

### 3- mongo-depl

This file has the *deployment* code of the Mongodb server along with *service* code.

- The docker container/Pod is exposed on port 27017.
- The pod is set for 1 replica.
- The server pod takes the Mongo-URL from Secret.
- The service type is *ClusterIP* and exposes port 27017 of the Pod.
- The pod uses Persistent Volume Claim (mongo-pvc) to store Mongodb data (/data/db).


### 4- mongo-secret

This file has the *secret* code of the Mongodb server by which the nodeJS server will use to connect to Mongodb server.

- The secret string stored is Mongodb URL (mongo-url: "mongodb://mongo-srv/habits").
Note: in real scenarios, you will use secret to store username and password therefore you should not commit it to your Github account.

### 5- mongo-pvc

This file has the code for creating the *PersistentVolumeClaim* resource.

- It requests the K8s node for 2Gi.
- The mode is set to *ReadWriteOnce* means one node can use it at a time.

### 6- ingress-srv

This file has the *ingress* service code.

- It uses the Nginx ingress controller.
- Hosting www.hamid.com (you can edit /etc/hosts file to trick it.)
- It uses regex to identify the request and route to a specific service (i.e client pod or server pod).
- Nginx ingress controller installed by the following link (https://kubernetes.github.io/ingress-nginx/deploy/#quick-start).



## Useful commands to check the resources after applying.

### To list all the objects in the default namespace

`kubectl get all -o wide`

### To list the services in the default namespace

`kubectl get svc -o wide`

### To list the pods in the default namespace

`kubectl get pods -o wide`

### To list the deployment in the default namespace

`kubectl get deployments -o wide`

### To get all the replicasets

`kubectl get replicasets -o wide`

### To get all the ingress list

`kubectl get ingress -o wide`

### To get all the secrets list

`kubectl get secrets -o wide`

### To get detail of the specific object.

`describe` argument is used to get details of any object. For example:
`kubectl describe <object> <name>`
