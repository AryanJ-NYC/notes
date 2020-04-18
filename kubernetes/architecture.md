# Architecture

At a high level, Kubernetes has the following main components:

* one or more **master nodes**
* one or more **worker nodes**
* distributed key-value store, such as **etcd**

![Kubernetes Architecture](../.gitbook/assets/image%20%286%29.png)

## Master Node

* provides a running environment for the **control plane** responsible for managing the state of a Kubernetes cluster
* **control plane** components are agents with very distinct roles in the cluster's management
* Users send requests to the master node via CLI, Web UI or API
* the control plane must run at all costs
  * to ensure control plane fault tolerance, master node replicas are added to the cluster configured in high-availability mode
* the cluster's configuration data is saved to [**etcd**](https://github.com/etcd-io), a distributed key-value store which only holds cluster state related data, no client workload data

### Components

#### API Server

* all administrative tasks are coordinated by the **kube-apiserver**, a central control plane component running on the master node
* the API server reads the Kubernetes cluster's current state from the etcd and, after a call's execution, the resulting state of the Kubernetes cluster is saved in the distributed key-value data store for persistence
  * it is the only master plane component to talk to the etcd data store

#### Scheduler

* the role of **kube-scheduler** is to assign new objects \(such as pods\) to nodes
* decisions are made based on the current Kubernetes cluster state and the new object's requirements
* the resource usage data and new object's requirements are received from the API server \(which fetches it from etcd\)

#### Controller Managers

* the **controller managers** are control plane components on the master node running controllers to regulate the state of the Kubernetes cluster
* they compare the cluster's desired state \(given the objects' configuration data\) with its current state \(obtained from etcd data store via API server\)
* the **kube-controller-manager** runs controllers responsible to act when nodes become unavailable
* the **cloud-controller-manager** runs controllers responsible to interact with the underlying infrastructure of a cloud provider when nodes become unavailable, to manage storage volumes provided by a cloud service and to manage load balancing and routing 

