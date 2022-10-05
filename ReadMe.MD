### 1. Creating a K8S Cluster

minikube start --nodes 2 -p multinode-cluster  --kubernetes-version=v1.23.0

Note: Check https://github.com/prometheus-operator/kube-prometheus#compatibility for the compability.

Kubernetes v1.23.0 is compatible with release-0.10 and release-0.11 of the kube-prometheus.


### Deleting the K8S Cluster
minikube delete -p multinode-cluster




