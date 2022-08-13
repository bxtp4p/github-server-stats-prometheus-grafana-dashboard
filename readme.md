# GitHub Server Stats Prometheus Grafana Dashboard

This repo contains a Grafana dashboard that can be used to monitor and visualize [GitHub Server Stats](https://docs.github.com/en/rest/enterprise-admin/admin-stats#get-github-enterprise-server-statistics) metrics. 

![image](https://user-images.githubusercontent.com/10562371/184500311-d54f90cf-53d6-4a17-964c-fb1de6a64fb4.png)


## Requirements

In order to use this dashboard in Grafana, you need to add as a data source a Prometheus server that is scraping the metrics exported by the [GitHub Server Stats Prometheus Exporter Action
](https://github.com/bxtp4p/github-server-stats-prom-exporter-action) and being sent to a [Prometheus Pushgateway](https://github.com/prometheus/pushgateway). The dashboard will show no data if the metrics haven't been collected yet.

## Usage

The [github-server-stats.json](./grafana/dashboards/github-server-stats.json) contains the Grafana dashboard. You can import this dashboard into an existing Grafana instance using the [Import Dashboard](https://grafana.com/docs/grafana/latest/features/dashboard/import) feature. 



If you don't have an existing Grafana / Prometheus / Pushgateway setup, you can use clone this repo and use it to stand one up in Docker Compose or Kubernetes, preloaded with the dashboard and Prometheus data sources.

### Docker Compose

```shell
docker compose -f docker-compose up -d 
```

Then open up a browser and go to http://localhost:3000/ to open the Grafana UI and use the dashboards.

### Kubernetes

1. Create ConfigMaps

```shell
kubectl create configmap grafana-provisioning-datasources --from-file=./grafana/etc/grafana/provisioning/datasources -o yaml
kubectl create configmap grafana-provisioning-dashboards --from-file=./grafana/etc/grafana/provisioning/dashboards  -o yaml
kubectl create configmap grafana-dashboards --from-file=./grafana/dashboards -o yaml
kubectl create configmap prometheus --from-file=./prometheus -o yaml
```

2. Create Pods and Services

```
cat ./kubernetes/*.yml | kubectl apply -f -
```
3. Access Grafana UI

```
kubectl port-forward svc/grafana 3000:3000
```


You can access the Grafana UI at http://localhost:3000/ to use the dashboards.
