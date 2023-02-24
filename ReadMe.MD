### 1. Creating a K8S Cluster

#### 1.1. On Local Machine using minikube
Run the following command to create a K8S cluster.

```minikube delete && minikube start --kubernetes-version=v1.23.0 --memory=6g --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.bind-address=0.0.0.0 --extra-config=controller-manager.bind-address=0.0.0.0```

#### 1.2. On AWS

Run the following command to create a K8S cluster named 'next-try-out-cluster' on AWS.

```eksctl create cluster --name next-try-out-cluster --version 1.23 --region eu-west-1 --nodegroup-name worker-nodes --node-type t2.large --nodes 2 --nodes-min 2 --nodes-max 3```


### 2. Installing Prometheus + Grafana using kube-prometheus-stack helm chart


#### 2.1. Add the prometheus-community helm repo
```helm repo add prometheus-community https://prometheus-community.github.io/helm-charts```
#### 2.2. Update the helm repo 
```helm repo update```
#### 2.3. Create a namespace called prometheus  
```kubectl create ns prometheus```
#### 2.4. Install the helm chart that include Prometheus + Grafana  
```helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus```
#### 2.5. Access the Dashboard of the Grafana by using port-forward
```kubectl -n prometheus port-forward deployment/prometheus-grafana 3000```

Then open a browser and access the dashboard with the address below:

```http://localhost:3000```

```default username : admin```

```default password: prom-operator```

#### 2.6.  Access the Dashboard of the Prometheus by using port-forward

```kubectl port-forward svc/prometheus-operated 9090 -n prometheus```

Then open a browser and access the dashboard with the address below:

```http://localhost:9090```

#### 2.7. Check the Service Monitors

ServiceMonitor declaratively specifies how groups of Kubernetes services should be monitored. 

```kubectl -n prometheus get servicemonitors```


#### 2.8. Install Helm Dashboard Plugin

The Helm Dashboard plugin offers a UI-driven way to view the installed Helm charts. Run the command below to install it:

```helm plugin install https://github.com/komodorio/helm-dashboard.git```

Start UI  with the following command:

```helm dashboard```


### Deleting the K8S Cluster

```eksctl delete cluster --region=eu-west-1 --name=next-try-out-cluster```


### Resources
1. https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack
2. https://github.com/prometheus-operator/kube-prometheus
3. [How to Monitor a Kubernetes Cluster in 2022 with Prometheus & Grafana - That DevOps Guy - Marcel Dempers](https://www.youtube.com/watch?v=YDtuwlNTzRc)
4. [Prometheus on Kubernetes: Day 28 of #100DaysOfKubernetes - Anais Urlichs](https://www.youtube.com/watch?v=n4eF8EcgpZQ) 
5. [Getting started with Prometheus Grafana and Node exporter - Part 1 - Rahul Wagh](https://www.youtube.com/watch?v=peH95b16hNI&list=PL7iMyoQPMtAPbN3A7Prac8vfikIPa02wd&index=1)
6. [ Prometheus Grafana dashboard for Kubernetes monitoring - Part 2 - Rahul Wagh](https://www.youtube.com/watch?v=3Xs49Urq16M&list=PL7iMyoQPMtAPbN3A7Prac8vfikIPa02wd&index=3) 
7. [A Beginner's Guide to Using the Prometheus Operator - Rodrigo Martinez](https://blog.container-solutions.com/prometheus-operator-beginners-guide)
8. [Helm Dashboard](https://github.com/komodorio/helm-dashboard)


