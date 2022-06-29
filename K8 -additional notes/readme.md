# Self-Study Material

## Table Of Contents
- [Kubernetes]()
- [Docker](#docker) 
  - [What Is Docker](#what-is-docker) 
  - [Why Use Docker](#why-use-docker)
# Kubernetes

## What Is Kubernetes
- The purpose of kubernetes is to host your applications in the form of containers in an automated fashion so that you can easily deploy as many instances of your application as required and easily enable communication.

## Overview
- Worker Nodes
  - Host application as containers
- Master  
  - Manage, plan, schedule and monitor nodes
- ETCD
  - Database that stores information in a key-value format
- Kube Scheduler
  - Identifies the right node to place a container on based on the containers resource requirements, the worker nodes capacity or any other policy or constraints
- Controller Manager
  - Node Controller
    - Takes care of nodes. Responsible for onboarding new nodes to the cluster and handling situation where nodes become unavailble or gets destroyed
  - Replication Controller
    - Ensures that the desired number of containers are running at all times in a replication group
- Kube API Server
  - Primary management component of kubernetes. Responsible for orchestrating all operations within the cluster. It exposes the kubernetes API which is used by external users to perform management operations on the cluster as well as the various controllers to monitor the state of the cluster and make necessary changes as required
- Kubelet
  - Agent that runs on each node in a cluster. It listens for instructions from the kube api server and deploys or destroyed containers on the node as required. Kube api server periodically fetches status reports from the kubelet to monitor the node and containers.
- Kube Proxy
  - ensures that the necessary rules are in place on the worker nodes to allow the containers running on them to reach each other

## Setting Up Kubernetes With ETCD
- Cluster from scratch 
  - Setup the ETCD manually
- Clutser with ETCD deployed in a Pod
  - `kubeadm`
- Specify different ETCD instances so they know about each other `initial-cluster`

## ETCDCTL
- Command-line interface tool used to interact with ETCD
- ETCDCTL can interact with ETCD Server using 2 API versions:
  - Version 2 (Default)
    - `etcdctl backup`
    - `etcdctl cluster-health`
    - `etcdctl mk`
    - `etcdctl mkdir`
    - `etcdctl set`
  - Version 3
    - `etcdctl snapshot save`
    - `etcdctl endpoint health`
    - `etcdctl get`
    - `etcdctl put`
- For version 3 set the environment variable ETCDCTL_API. If this is not set it is assumed to be set to version 2
  - `export ETCDCTL_API=3`

## Basic Commands
- `kubectl get pods -n kube-system`
- `kubectl run nginx --image nginx`
- `kubectl get pods`
- `kubectl get pods -o wide`
- `kubectl delete pod <podname>`
- `kubectl describe pod <podname>`
- `kubectl run <newpodname> --image=<imagename> --dry-run=client -o yaml > <yamlfilename>.yaml`
- `kubectl get replicaset`
- `kubectl get replicationcontroller`
- `kubectl edit rs <replicesetname>`
- `kubectl scale --replicas=6 -f <yamlfilename>.yml`
- `kubectl create deployment <deploymentname> --image=<imagename> --replicas=<numberofreplicas>`

## YAML In Kubernetes
- Structure of the YAML creating a pod:
```
apiVersion: Version of the kubernetes api to create the object
kind: type of object we are trying to create
"Kind = Version(Pod = v1, Service = v1, ReplicaSet = apps/v1 and Deployment = apps/v1)"
metadata: Data about the object such as:
  name: myapp
  labels: (You can add any key-value pair in the label dictionary)
    app: myapp

spec: (This is where you provide additional information to kubernetes about the object)
  containers: (list/Array)
    - name: nginx-container
      image: nginx
``` 
- Structure of the YAML creating a replicaset:
```
apiVersion:
kind:
metadata:
spec:
  template:
  replicas:
  selector:
```
- `kubectl create -f <yamlfilename>.yml`
- `kubectl replace -f <yamlfilename>.yml`

# Docker
## What Is Docker 
- Docker is a platform or ecosystem around creating and running containers
## Why Use Docker 
- Docker makes it really easy to install and run software without worrying about setup or dependencies
## Docker Terminology
- Image - A single file with all the dependencies and configurations required to run a program
- Container - Instance of an image. "A container is a program with its own isloated set of hardware"
- Docker CLI (Docker Client) - Tool that we issue commands to help us interact with Docker Daemon 
- Docker Daemon (Docker Server) - Tool that is responsible for creating images, running containers, etc 
- STDIN (Input from your terminal), STDOUT (Output to your terminal), STDERR (Error messages from the program)
## Docker Commands
- ```docker system df``` - analyze how much space Docker is using
- ```docker system prune -a``` - clean up everything in docker
- ```docker image prune``` - clean up unused and dangling images
- ```docker image prune -a``` - clean up dangling images only
- ```docker container prune``` - clean up stopped containers
- ```docker volume prune``` - clean up unused volumes
- ```docker run <image name>``` - run a container "docker run = docker create + docker start"
- ```docker ps``` - list all running containers
- ```docker ps --all``` - list all containers that are running and that have been stopped
- ```docker logs <container id>``` - see all logs that have been emitted by a container
- ```docker stop <container id>``` - stops a container "SIGTERM" gives time to the container to shutdown (10 seconds)
- ```docker kill <container id>``` - stops a container instantly "SIGKILL" does not give time to the container to shutdown
- ```winpty docker exec -it <container id> <command>``` - execute an additional command in a container. -i connects to the STDIN and -t formats the information nicely
- ```winpty docker exec -it <container id> sh``` - command prompt in a container. zsh, bash, powershell are also other command prompts you can use.
- ```docker run -it <container image> sh``` - start a container with a shell
## Creating A Dockerfile
- Specify A Base Image > Run Some Commands To Install Additional Programs > Specify A Command To Run On Container Startup
- ```docker build .```