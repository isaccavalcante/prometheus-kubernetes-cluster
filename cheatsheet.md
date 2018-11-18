# Commands cheatsheet

Run prometheus on docker container mounting configuration file:

```sh
docker run -d -p 9090:9090 -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```

Run grafana on docker container:

```sh
docker run -d -p 3000:3000 grafana/grafana
```

Create kubernetes pod from configuration file:

```sh
kubectl create -f pod.yml
```

List created pods:

```sh
kubectl get pods 
```