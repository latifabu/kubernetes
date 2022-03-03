## Kubernetes
### What is Kubernetes
- Kubernetes, also known as K8s, It is known as a container orchestration tool
- It is an an open-source system for automating deployment, scaling, and management of containerized applications
- It's responsible for allocating and scheduling containers and providing then with abstracted functionality.
- Responsible for health checks, file storage
- In summary k8 is about controlling how, when and where containers are run.
  
### The benefits
- Keeps a check on the health of the nodes and containers.
- Requires less resources
- Automated rollouts and rollback
- **Auto Scaling**, horizontal scaling: scales application up and down quickly either manually or automatically
- **Self Healing** - kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Storage orchestration** - Kubernetes allows users to automatically mount a storage system of choice, such as local storage, public cloud providers, and more. You still need to provide the underlying storage system.
-**Automatic bin packing** - Kubernetes places containers automatically based on the required resources and other restrictions without impairing availability. Automated rollouts and rollbacks: Kubernetes distributes changes to the software or the configuration using a rollout.
- **Secret and configuration management** storage and management of sensitive information, such as passwords and SSH keys. Secrets can be updated, deployed and application configuration without rebuilding your container images. Without exposing secrets in your stack configuration.




### Use cases
![k8 use cases](https://user-images.githubusercontent.com/98215575/156419629-672a7567-6a83-4651-a040-9f9422ed9ed9.jpg)

Kubernetes and Amazon EC2: 
- Kubernetes manage clusters for Amazon EC2, compute instances as well as run containers on these instances using processes of deployment, maintenance, and scaling.
- AWS has collaborated with Kubernetes’ community and making contributions to its code-base 
- This collaboration allows users to fully manage their Kubernetes deployment and utilize a power

The New York Times:
- One of the biggest publications in the world.
- They wanted to avoid sending out incorrect and unverified news to their readers. 
- The company decided to move out its data centers and less critical applications that shall be managed on VMs.
- Kubernetes offered its solution (Google Kubernetes Engine )GKE
- There was a significant increase in the spread, and deployment, and productivity
- NYT has now moved on from a ticket-based system to requesting resources that deploy weekly schedules.

### How does it benefit the business
- Portability across on-prem, public clouds and multi-cloud capability.
- Collaboration, unifis development and operational teams on a single platform.
- Deploy live changes for users
- Increased developer productivity


![k8 benefits](https://user-images.githubusercontent.com/98215575/156419278-7f283df7-258d-4f8e-bdd0-601a89aad016.png)



### k8 architecture 
- When K8 is deployed it creates a cluster. 
- A cluster contains machines called nodes that host containerised applications.
- The control plane oversees nodes and pods in each cluster.

![architecture](https://user-images.githubusercontent.com/98215575/156420651-a5cb6978-305d-42d5-8953-1b9a3b454d61.png)


- **Nodes**:node is a virtual or physical machine that runs workloads. Each node contains the services necessary to run pods:
- **Kubelet**	An agent that runs on each node in a cluster. It ensures that the containers running in a pod are in a healthy state.
- **Pods**	A pod consists of containers and supplies shared storage, network resources and specifies how the containers will run.
- **Control Plane**	The Control Plane is responsible for maintaining the desired end state of the Kubernetes cluster as defined by the developer.
- **K-proxy**	A network proxy service that runs on each node in a given cluster. It maintains network rules on nodes that allow network communications to pods from within or outside the cluster.



### K8 roll back - how to use it
- Rolling updates are the default strategy to update the running version of your app.
- When newer version of your app and Kubernetes makes sure that the rollout happens without disrupting the live traffic.
- Rolling updates still have risks, application can perform unexpectedly after deployment.
- When a change that breaks production a roll back plan can be enforced by Kubernetes.
- Kubernetes has a built-in mechanism for rollbacks.
- Kubernetes and `kubectl` offer a simple mechanism to roll back changes to resources such as Deployments, StatefulSets and DaemonSets.
- Kubernetes keeps a rolling revision history for pods, so you can roll back to any stored revision (the number of revisions is configurable with a default value of 10).


### Kubernetes commands
- `kubectl get name-service - deploy/deployment - service/svc - pods`
- `kubectl create -f file.yml` e.g. `kubectl create -f nginx-deploy.yml`
-`kubectl delete name-service deploy deploy-name` e.g `kubectl delete pods nginx-deployment-7d9b964898-5w88w `
- `kubectl get service` or `kubectl get svc `for kubernetes service information
- To see pods running: `kubectl get pods`
- `kubectl` for official documentation
- `kubectl explain svc`
- `kubectl delete all --all` very dangerous 
- `kubectl get all` - will name the pods, the status and if they are ready
- `kubectl get pods`
- `kubectl describe pods (nameofmongopod)` - can see pvc, image id etc, detailed infor
- `kubectl delete pvc --all` deletes all persistent variables
- `kubectl delete pvc [name]` - specific pvc
- `kubectl delete all --all` very dangerous 
 ### Creating pods:

 Nginx-deploy.yml:

```
---
# k8 is a yml file -
# K8s works with API versions to declare the resources
# we are going to create a deployment for our nginx-image
# we will create 3 pods with this deployment
# kubectl get name-service - deploy/deployment - service/svc - pods
# kubectl create -f file.yml
# kubectl delete name-service deploy deploy-name

apiVersion: apps/v1 # which api call we need to make for k8 to make a deployment for us
kind: Deployment # pod, service # replicaset # ASG
metadata: # metadata to name your deployment - case insensitive
  name: nginx-deployment

spec: # labels and selectors are communication services between micro-services
  selector:
    matchLabels:
      app: nginx # look for this label to match with k8s service

  replicas: 3

# template to use it's label for k8s to launch in the browser
  template:
    metadata:
      labels:
        app: nginx

# define the container specs
    spec:
      containers:
      - name: nginx
        image: latifsparta/latif_nginx   # ahskhan/sre_nginx_test
        ports:
        - containerPort: 80

        imagePullPolicy: Always

```

Nginx-svc.yml:
- Map our localhost to port 80 using loadbalancers
  
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-deployment
  namespace: default
  resourceVersion: "40883"
  uid: 9190ab75-d61c-4ff4-a3d1-0d293fa8d72e
#  creatinTimestamp: 

spec:
  ports:
  - nodePort: 30442
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  sessionAffinity: None
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
      - hostname: localhost  
```
- **nodePort** - A NodePort service is the most primitive way to get external traffic directly to your service. NodePort, as the name implies, opens a specific port on all the Nodes (the VMs), and any traffic that is sent to this port is forwarded to the service."By default, the range of the service NodePorts is 30000-32768. This range contains 2768 ports, which means that you can create up to 2768 services with NodePorts."
- **Load balancer** - A LoadBalancer service is the standard way to expose a service to the internet. On GKE, this will spin up a Network Load Balancer that will give you a single IP address that will forward all traffic to your service.

### Deploying our app using Kubernetes

```
---
apiVersion: apps/v1 #which api call we need to make for k8 to make a deployment for us
kind: Deployment #tool for running local K8 clusters using Docker container "node"
metadata: # metadate to name your deployment - case insenstive
  name: node-deployment
spec: # labels are selectors are communication services between micro-services
  selector:
    matchLabels:
      app: node
  
  replicas: 3 # how many pods
  template: # template to use it's label for k8s to launch in the browser
    metadata:
      labels:
        app: node # visible name of pods 
    spec:
      containers:
        - name: node
          image: latifsparta/dockerapp:v2 #my image 
          ports: 
            - containerPort: 3000
          env:
            - name: DB_HOST
              value: mongodb://mongo:27017/posts
          imagePullPolicy: IfNotPresent  # if image is not present on dockerhub, will pull from localhost

---
apiVersion: v1
kind: Service
metadata:
  name: node
spec:
  selector:
    app: node
  ports:
    - port: 3000
      targetPort: 3000
  type: LoadBalancer     
```
### Mongodb deployment and persistent volume claim

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo
spec:
  selector:
    matchLabels:
      app: mongo
  replicas: 1
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
        - name: mongo
          image: mongo
          ports:
            - containerPort: 27017
          volumeMounts:
            - name: storage
              mountPath: /data/db
      volumes:
        - name: storage
          persistentVolumeClaim:
            claimName: mongo-db

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongo-db
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 256Mi
---
apiVersion: v1
kind: Service
metadata:
  name: mongo
spec:
  selector:
    app: mongo
  ports:
    - port: 27017
      targetPort: 27017
```
### Volumes

- When restarting a pod, the data is not persistent by default.
- You have to configure the database to be persistent by using volumes.
- Volumes save the data even when the pod is gone.
- New pods will be able to read exisiting data from the storage
- The storage is highly avaliable
- Can be accessed by all pods
- Importance of volume, storage that does not depend on the pod lifecycle
  
#### Another benefit:
- When auto-scaling comes into play
- When a pod is destroyed and new ones spin up
- The new nodes can access the volume
- This cannot be done if the data is not persistent


