### 1. Creating a K8S Cluster

#### 1.1. On Local Machine using kind
Run the following command to create a K8S cluster.

```kind create cluster --config kind-config.yaml```

#### 1.2. On AWS

Run the following command to create a K8S cluster named 'next-try-out-cluster' on AWS.

```eksctl create cluster --name next-try-out-cluster --version 1.23 --region eu-west-1 --nodegroup-name worker-nodes --node-type t2.large --nodes 2 --nodes-min 2 --nodes-max 3```


### 2. Installing Prometheus + Grafana using kube-prometheus-stack helm chart


#### 2.1. Add the prometheus-community helm repo
```helm repo add prometheus-community https://prometheus-community.github.io/helm-charts```
#### 2.2. Update the helm repo 
```helm repo update```
#### 2.3. Create a namespace called monitoring  
```kubectl create ns monitoring```
#### 2.4. Install the helm chart that include Prometheus + Grafana  
```helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring```
#### 2.5. Access the Dashboard of the Grafana 
#### 2.5.1 Using port-forward: 
```kubectl -n monitoring port-forward deployment/prometheus-grafana 3000```

Open a browser and access the Grafana dashboard via address:
```http://localhost:3000```

or

#### 2.5.2 Using load balancer: 

2.5.2.1 Patch the Service to use a cloud load balancer using the command below:

```kubectl patch svc prometheus-grafana -n monitoring -p '{"spec": {"type": "LoadBalancer"}}'```

Note: The Load Balancer can take a couple of minutes in being available on the DNS.

2.5.2.2 Run ```kubectl get svc prometheus-grafana -n monitoring``` and get EXTERNAL-IP.

2.5.2.3 Open a browser and access the Grafana dashboard via address EXTERNAL-IP.

or

#### 2.5.3 Using NodePort: 

2.5.3.1 Patch the Grafana service to use a node port using the command below:

```kubectl patch svc prometheus-grafana -n monitoring -p '{"spec": {"type": "NodePort", "ports": [{"name": "web", "nodePort": 31555, "port": 80}]}}'```

2.5.3.2 Run ```kubectl get nodes -o wide``` and get INTERNAL-IP of the master node.

2.5.3.3 Open a browser and access the Grafana dashboard via address http://INTERNAL-IP:31555


Note: The default username and password for Grafana dashboard are:

```default username : admin```

```default password: prom-operator```

#### 2.6.1  Access the Dashboard of the Prometheus by using port-forward

```kubectl port-forward svc/prometheus-kube-prometheus-prometheus 9090 -n monitoring```

Then open a browser and access the dashboard with the address below:

```http://localhost:9090```

#### 2.6.2  Access the Dashboard of the Prometheus by using NodePort

2.6.3.1 Patch the Prometheus service to use a node port using the command below:

```kubectl patch svc prometheus-kube-prometheus-prometheus -n monitoring -p '{"spec": {"type": "NodePort", "ports": [{"name": "web", "nodePort": 31666, "port": 9090}]}}'```

2.6.3.2 Run ```kubectl get nodes -o wide``` and get INTERNAL-IP of the master node.

2.6.3.3 Open a browser and access the Prometheus dashboard via address http://INTERNAL-IP:31666

#### 2.7. Check the Service Monitors

ServiceMonitor declaratively specifies how groups of Kubernetes services should be monitored. 

```kubectl -n monitoring get servicemonitors```


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
9. [Exposing K8S Service](https://archive.eksworkshop.com/beginner/130_exposing-service/exposing/)
10. https://medium.com/@charled.breteche/kind-fix-missing-prometheus-operator-targets-1a1ff5d8c8ad


