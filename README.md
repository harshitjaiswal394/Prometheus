# KubernetesMonitoring using Prometheus and Grafana



Prometheus Architecture

![image](https://github.com/KarthiCholan/KubernetesMonitoring/assets/108706606/2a637cbe-d8c0-4247-8b96-77119c28f372)



What is Prometheus?

Prometheus is an open source monitoring tool
Provides out-of-the-box monitoring capabilities for the Kubernetes container orchestration platform. It can monitor servers and databases as well.
Collects and stores metrics as time-series data, recording information with a timestamp 
It is based on pull and collects metrics from targets by scraping metrics HTTP endpoints.

What is Grafana?

Grafana is an open source visualization and analytics software. 
It allows you to query, visualize, alert on, and explore your metrics no matter where they are stored.

Why System monitoring is critical?

Alerting: can give early warning of issues or problems before they become serious, costly or irreversible.

Visibility: Monitoring provides real-time insights into the health and performance of your applications and infrastructure.

Capacity Planning: By collecting and analyzing data over time, you can make informed decisions about capacity planning, such as when to add or remove nodes, allocate more resources, or scale your applications.






#  Prequisities

AWS EKS Cluster

HELM Should be installed



To bring up monitoring stack using docker 

#### 1. Prometheus

```
docker run -d --name prometheus-container -v /home/sndee/prometheus.yml:/etc/prometheus/prometheus.yml -e TZ=UTC -p 9090:9090 ubuntu/prometheus:2.33-22.04_beta
```

Note:- For more info [prometheus](https://hub.docker.com/r/ubuntu/prometheus)

#### 2. Grafana

```
docker run -d --name=grafana -p 3000:3000 grafana/grafana:8.5.5
```

Note:- For more info [Grafana](https://hub.docker.com/r/grafana/grafana)

#### 3. InfluxDb

```
docker run -d -p 8086:8086 --name influxdb2 influxdb:1.8.6-alpine
```

**Connect to container and execute below Influx Commands**
```
docker exec -it influxdb2 bash 
influx
CREATE DATABASE "jenkins" WITH DURATION 1825d REPLICATION 1 NAME "jenkins-retention"
SHOW DATABASES
USE <database_name>
```

![image](https://user-images.githubusercontent.com/29688323/218266342-da04d428-5e2f-4986-a68e-1d3789046eb5.png)

**Alerts**
```
docker run -d --name prometheus-alertmanager-container -e TZ=UTC -v /home/sndee/alertmanager.yml:/etc/alertmanager/alertmanager.yml -p 9093:9093 ubuntu/prometheus-alertmanager:0.23-22.04_beta
```

```
docker run -d --name prometheus-container -v /home/sndee/prometheus.yml:/etc/prometheus/prometheus.yml -v /home/sndee/prometheus_rules.yml:/etc/prometheus/prometheus_rules.yml -e TZ=UTC -p 9090:9090 ubuntu/prometheus:2.33-22.04_beta
```

```
SELECT count(build_number) FROM ( SELECT * FROM  "jenkins_data" WHERE ("project_name" =~ /^(?i)admin-api$/ AND "project_path" =~ /.*(?i)admin.*$/) ORDER BY time DESC LIMIT 10 ) WHERE ("build_result" = 'FAILURE' OR "build_result" = 'CompletedError' ) ORDER BY time DESC
```

```
docker run -d --name=grafana -p 3000:3000 -v /home/sndee/grafana.ini:/etc/grafana/grafana.ini grafana/grafana:8.5.5
```
####  4. Final Dashboard ----Easily monitor your deployment of Jenkins metrics with Grafana and Prometheus


<img width="960" alt="grafana monitor-jenkins" src="https://github.com/harshitjaiswal394/Prometheus/assets/116247746/01c820ba-2ceb-4419-8544-a96504b5a158">



