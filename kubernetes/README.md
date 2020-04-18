---
description: >-
  Kubernetes means helmsman or ship pilot (Kubernetes is the pilot on a ship of
  containers).
---

# Kubernetes

## Features

### Automatic bin packing

Kubernetes automatically schedules containers based on resource needs and constraints to maximize utilization without sacrificing availability

### Self-healing

Kubernetes automatically replaces and reschedules containers from failed nodes. It kills and restarts containers unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.

### Horizontal Scaling

Kubernetes applications are scaled manually or automatically based on CPU or custom metrics utilization.

### Service discovery and Load balancing

Containers receive their own IP addresses from Kubernetes while it assigns a single Domain Name System \(DNS\) name to a set of containers to aid in load-balancing requests across the containers of the set.

### Automated rollouts and rollbacks

Kubernetes seamlessly rolls out and rolls back application updates and configuration changes, constantly monitoring the application's health to prevent any downtime.

### Secret and configuration management

Kubernetes manages secrets and configuration details for an application separately from the container image, in order to avoid a re-build of the respective image. Secrets consist of confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.

### Storage Orchestration

Kubernetes automatically mounts software-defined storage \(SDS\) solutions to containers from local storage, external cloud providers, or network storage systems.

### Batch Execution

Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

