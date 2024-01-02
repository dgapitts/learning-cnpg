
# Base cluster / control plane setup via kind

To start a new cluster `kind create cluster --name dbcluster01`  [see here](01-Background-installs.md) for base setup details 


```
~/projects/learning-cnpg $ kubectl cluster-info --context kind-dbcluster01
Kubernetes control plane is running at https://127.0.0.1:62875
CoreDNS is running at https://127.0.0.1:62875/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```




# Base cnpg cluster database setup (3node cluster)

```
helm repo add prometheus-community   https://prometheus-community.github.io/helm-charts
helm upgrade --install   -f https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/main/docs/src/samples/monitoring/kube-stack-config.yaml   prometheus-community   prometheus-community/kube-prometheus-stack
kubectl apply -f kubectl apply -f cluster-example.yaml
```

and

```
~/projects/learning-cnpg $ kubectl get pods | grep -v prometheus
NAME                                                       READY   STATUS    RESTARTS   AGE
cluster-with-metrics-1                                     1/1     Running   0          59m
cluster-with-metrics-2                                     1/1     Running   0          59m
cluster-with-metrics-3                                     1/1     Running   0          59m

```

# Add montioring (prometheus and grafana)


To add prometheus and grafana working, I followed the [doco here](https://cloudnative-pg.io/documentation/current/quickstart/#part-4-monitor-clusters-with-prometheus-and-grafana)


```
~/projects/learning-cnpg $ kubectl apply -f PodMonitor.yaml
podmonitor.monitoring.coreos.com/cluster-example created
```

you need `port-forward` operations for both prometheus and grafana
```
~/projects/learning-cnpg $  kubectl port-forward svc/prometheus-community-kube-prometheus 9090
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
Handling connection for 9090
Handling connection for 9090
Handling connection for 9090
```
and
```
~/projects/learning-cnpg $ kubectl port-forward svc/prometheus-community-grafana 3000:80
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
Handling connection for 3000
Handling connection for 3000
Handling connection for 3000
```



There is also a pre-defined dasbboard 

```
kubectl apply -f \
>   https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/main/docs/src/samples/monitoring/grafana-configmap.yaml
configmap/test-cnp-dashboard created
```

