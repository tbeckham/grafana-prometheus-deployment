# Monitoring Infrastructure Deployment 
Repository for monitoring infrastructure based off of grafana+prometheus containers deployed using docker-compose. The ```default-deployment.yml``` Compose file deploys the following container applications:

* [grafana](https://grafana.com/)
* [prometheus](https://prometheus.io/)
* [node-exporter](https://github.com/prometheus/node_exporter)
* [cadvisor](https://github.com/google/cadvisor)

An additional Compose file has been created - ```grafana-mysqld-deployment.yml``` - deploys the same environment, along with the following container applications:

* [mysql-server](https://hub.docker.com/r/mysql/mysql-server/)
* [mysqld-exporter](https://github.com/prometheus/mysqld_exporter)

The second deployment provides better performance for Grafana when using a large number of dashboards.

## Prerequisites

* [Docker 1.13 or higher](https://docs.docker.com/engine/installation/)
* [Docker Compose](https://docs.docker.com/compose/overview/)

## Deployment

1. Clone repository:
```
$ git clone https://github.com/hspencer77/grafana-prometheus-deployment.git
```
2. Change directory into the repository, then deploy:
```
$ cd grafana-prometheus-deployment
$ docker-compose -f default-deployment.yml up --build -d
....
Creating monitoring-node-exporter ...
Creating monitoring-cadvisor ...
Creating monitoring-cadvisor
Creating monitoring-cadvisor ... done
Creating monitoring-prometheus ...
Creating monitoring-prometheus ... done
Creating monitoring-grafana ...
Creating monitoring-grafana ... done
$ docker ps
CONTAINER ID        IMAGE                                 COMMAND                  CREATED             STATUS                    PORTS                    NAMES
543e40407292        grafanaprometheusdeployment_grafana   "/usr/local/bin/gr..."   25 seconds ago      Up 24 seconds (healthy)   0.0.0.0:80->3000/tcp     monitoring-grafana
05487d4a1ee2        prom/prometheus                       "/bin/prometheus -..."   36 seconds ago      Up 35 seconds (healthy)   0.0.0.0:9090->9090/tcp   monitoring-prometheus
988f4a3554bd        google/cadvisor                       "/usr/bin/cadvisor..."   37 seconds ago      Up 36 seconds (healthy)   8080/tcp                 monitoring-cadvisor
4556c4f46b6f        prom/node-exporter                    "/bin/node_exporte..."   37 seconds ago      Up 36 seconds (healthy)   9100/tcp                 monitoring-node-exporter
```
3. Open a browser on the machine running the containers, and navigate to the following URL:
```
http://localhost/login
```
The default username is ```admin```.  The default password (which can be found in the ```config.monitoring``` or ```config.monitoring.mysql``` file as the value for the GF_SECURITY_ADMIN_PASSWORD variable) is ```foobar```. 

There are default dashboards associated with each deployment that show graphs and metrics for the monitoring infrastructure containers.  Metrics in Prometheus can be viewed directly by going to the following URL:
```
http://localhost:9090/graph
```

## Clean Up

To destroy the deployment, run the following command:
```
$ docker-compose -f default-deployment.yml down --rmi all -v
Stopping monitoring-grafana ... done
Stopping monitoring-prometheus ... done
Stopping monitoring-cadvisor ... done
Stopping monitoring-node-exporter ... done
Removing monitoring-grafana ... done
Removing monitoring-prometheus ... done
Removing monitoring-cadvisor ... done
Removing monitoring-node-exporter ... done
Removing network grafanaprometheusdeployment_front-tier
Removing network grafanaprometheusdeployment_back-tier
Removing volume grafanaprometheusdeployment_prometheus_data
Removing volume grafanaprometheusdeployment_grafana_data
Removing image prom/node-exporter
Removing image google/cadvisor
Removing image prom/prometheus
Removing image grafanaprometheusdeployment_grafana
```
