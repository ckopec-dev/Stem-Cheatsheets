
# Kubernetes Cheatsheet

## Overview

- K8s: shorthand for Kubernetes
- Purpose: maintain, update, and monitor containers
- Cluster: set of connected computers (Nodes)
- Control Plane: manages nodes
- Nodes: worker machines running Kubelet (typically on Linux in a Docker container)
- Pods: smallest deployable unit. Consists of 1 or more containers
- Services: exposes network connectivity
- Kubectl: main CLI for managing K8s objects (pods, services, etc.)

## Installation

[Official guide with current commands](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

~~~bash
# Download
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
$ curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

# Validate
$ echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

# Install and confirm
$ sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
$ kubectl version --client
~~~

## Install minikube (for learning and devlopment)

[Official guide)[https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download]

~~~bash
# Download
$ curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

# Install
$ sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

# Start cluster and confirm
$ minikube start
$ kubectl get po -A

# Alias command
$ alias kubectl="minikube kubectl --"
~~~

## Persistent volumes

- Fundamental objects for persistent storage, maintained in parallel to pods
- Mapped to pods using Persistent Volume Claims (PVCs)
- Storage Classes allow dynamic autonomous provisioning

### Common storage commands

~~~bash
# All available storage classes
$ kubectl get sc

# All deployed PVCs
$ kubectl get pvc

# All deployed persistent volumes
$ kubectl get pv
~~~

## Networking

- Load balancer: distributes network requests across all pods in a Service
- Ingress: objects used to route traffic from outside the cluster to services inside the cluster. Rules define which requests are served by which service. Usually used in conjunction with load balancing.

## Data pipelines

- Set of steps to move, transform, or analyze data
- ETL: Extract data from sources, transform it into a useful schema, and load it into a target (often a database)
- ELT: Extract data from sources, load into a target, and transform it into a useful schema
