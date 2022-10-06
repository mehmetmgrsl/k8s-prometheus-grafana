### 1. Creating a K8S Cluster

Run the following command to create cluster.

```
minikube delete && minikube start --kubernetes-version=v1.23.0 --memory=6g --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.bind-address=0.0.0.0 --extra-config=controller-manager.bind-address=0.0.0.0
```

* Kubernetes v1.23.0 is compatible with release-0.10 and release-0.11 of the kube-prometheus.

* Check https://github.com/prometheus-operator/kube-prometheus#compatibility for the compability.

### 2. Installing Prometheus + Grafana using kube-prometheus-stack helm chart


#### 2.1. Add the prometheus-community helm repo
```helm repo add prometheus-community https://prometheus-community.github.io/helm-charts```
#### 2.2. Update the helm repo 
```helm repo update```
#### 2.3. Create a namespace called prometheus  
```kubectl create ns prometheus```
#### 2.4. Install the helm chart that include Prometheus + Grafana  
```helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus```
#### 2.5. Access the Dashboard of the Grafana by using port-forward.
```kubectl -n prometheus port-forward deployment/prometheus-grafana 3000```
#### 2.6. Open a browser and access the dashboard with the address below:
```http://localhost:3000```

```default username : admin```

```default password: prom-operator```


### Deleting the K8S Cluster
minikube delete


### Resources

1. https://github.com/prometheus-operator/kube-prometheus
2. [How to Monitor a Kubernetes Cluster in 2022 with Prometheus & Grafana](https://www.youtube.com/watch?v=YDtuwlNTzRc)
3. https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack 
4. 


