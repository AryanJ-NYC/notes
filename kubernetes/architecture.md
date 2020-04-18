# Architecture

At a high level, Kubernetes has the following main components:

* one or more **master nodes**
* one or more **worker nodes**
* distributed key-value store, such as **etcd**

![Kubernetes Architecture](../.gitbook/assets/image%20%286%29.png)

### Master Node

* provides a running environment for the **control plane** responsible for managing the state of a Kubernetes cluster
* **control plane** components are agents with very distinct roles in the cluster's management
* Users send requests to the master node via CLI, Web UI or API
* the control plane must run at all costs
  * to ensure control plane fault tolerance, master node replicas are added to the cluster configured in high-availability mode
* the cluster's configuration data is saved to [**etcd**](https://github.com/etcd-io), a distributed key-value store which only holds cluster state related data, no client workload data

