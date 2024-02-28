# Execute the below command to add the Helm Stable Charts for your local client

helm repo add stable https://charts.helm.sh/stable



# Add prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts


# Search Repo
helm search repo prometheus-community

# Create Prometheus namespace
kubectl create namespace prometheus


# Install kube-prometheus-stack

helm install stable prometheus-community/kube-prometheus-stack -n prometheus



# Check if prometheus and grafana pods are running already
kubectl get pods -n prometheus


kubectl get svc -n prometheus



# To make prometheus and grafana available outside the cluster, use LoadBalancer or NodePort instead of ClusterIP.

# Edit Prometheus Service

kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus



# Edit Grafana Service

kubectl edit svc stable-grafana -n prometheus


kubectl get svc -n prometheus


![Prometheus](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/038af60d-fca4-422e-8dfb-cfd0c4cf8111)



# Access Grafana UI in the browser

Get the URL from the above screenshot and put in the browser


![image](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/c05a919b-62ac-4697-b710-60ad44c7d63b)





# Import the dashboard




![image](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/02c71114-393d-4967-aa30-75bcc2417e6a)





# Container Monitoring Dashobard --12740 


![image](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/ac2c72cf-0e79-4f5e-9710-6c8671d25623)



# POD Monitoring Dashboard --6417


![image](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/ce46b32d-c99b-4ab9-8dd6-6797ecb40f29)

 