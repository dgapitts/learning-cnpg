
# Base cluster / control plane setup via kind

To start a new cluster `kind create cluster --name dbcluster01`  [see here](01-Background-installs.md) for base setup details 


```
~/projects/learning-cnpg $ kubectl cluster-info --context kind-dbcluster01
Kubernetes control plane is running at https://127.0.0.1:62875
CoreDNS is running at https://127.0.0.1:62875/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```




# Helm charts for prometheus-community   prometheus-community/kube-prometheus-stack

First add the helm charts and install the prometheus-community   prometheus-community/kube-prometheus-stack

```
helm repo add prometheus-community   https://prometheus-community.github.io/helm-charts
helm upgrade --install   -f https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/main/docs/src/samples/monitoring/kube-stack-config.yaml   prometheus-community   prometheus-community/kube-prometheus-stack
```

and I also added a dashboard 

```
kubectl apply -f   https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/release-1.22/releases/cnpg-1.22.0.yaml
```


```


```
~/projects/learning-cnpg $ kubectl apply -f PodMonitor.yaml
podmonitor.monitoring.coreos.com/cluster-example created
```


and

```
~/projects/learning-cnpg $ kubectl get pods | grep -v prometheus
NAME                                                       READY   STATUS    RESTARTS   AGE
cluster-with-metrics-1                                     1/1     Running   0          59m
cluster-with-metrics-2                                     1/1     Running   0          59m
cluster-with-metrics-3                                     1/1     Running   0          59m

```

## Add montioring (prometheus and grafana) - port-forward svc


To add prometheus and grafana working, I followed the [doco here](https://cloudnative-pg.io/documentation/current/quickstart/#part-4-monitor-clusters-with-prometheus-and-grafana)



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
kubectl apply -f https://raw.githubusercontent.com/cloudnative-pg/cloudnative-pg/main/docs/src/samples/monitoring/grafana-configmap.yaml
configmap/test-cnp-dashboard created
```




## Appendix A - debugging notes

### failed to call webhook 
Regarding this error, I think the fix was just a question of being patient

```
~/projects/learning-cnpg $ kubectl apply -f cluster-example.yaml
Error from server (InternalError): error when creating "cluster-example.yaml": Internal error occurred: failed calling webhook "mcluster.cnpg.io": failed to call webhook: Post "https://cnpg-webhook-service.cnpg-system.svc:443/mutate-postgresql-cnpg-io-v1-cluster?timeout=10s": dial tcp 10.96.147.169:443: connect: connection refused
~/projects/learning-cnpg $ kubectl get deployment -n cnpg-system cnpg-controller-manager
NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
cnpg-controller-manager   1/1     1            1           43s
~/projects/learning-cnpg $ kubectl apply -f cluster-example.yaml
cluster.postgresql.cnpg.io/cluster-with-metrics created
```



### cluster-with-metrics missing in the dashboard

I updated the  PodMonitor.yaml with `enablePodMonitor: false`

```
~/projects/learning-cnpg $ vi PodMonitor.yaml
~/projects/learning-cnpg $  kubectl apply -f PodMonitor.yaml
cluster.postgresql.cnpg.io/cluster-with-metrics configured
```


and switched back to `enablePodMonitor: true`
```
~/projects/learning-cnpg $ vi PodMonitor.yaml
~/projects/learning-cnpg $  kubectl apply -f PodMonitor.yaml
cluster.postgresql.cnpg.io/cluster-with-metrics configured
```
