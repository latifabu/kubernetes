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