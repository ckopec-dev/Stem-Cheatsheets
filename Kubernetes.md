
# Kubernetes Cheatsheet

## Installation

[Official guide with current commands](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

~~~
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

~~~
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
