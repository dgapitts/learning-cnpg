# wrapper script check_logs


As per this [cloudnativepg slack thread](https://cloudnativepg.slack.com/archives/C03AX0J5P29/p1704300346034549) where I was trying to debug "custom monitoring queries" deployment issues:
* I needed to debug all the logs
* the cnpg plugin has a log aggregator (but that is an extra install)
* but I like this script as it is to see which LOG file the message is in - at least for debugging small test cluster


```
kubectl get pods| grep -v NAME| awk '{print "echo LOGS for " $1 " : ; kubectl logs " $1 }' | tee check_logs.sh
bash check_logs.sh > check_logs.txt
```


```
vi check_logs.txt
```

```
grep 'LOGS\|custom' check_logs.txt
```

for example 

```
LOGS for cluster-with-metrics-1 :
{"level":"info","ts":"2024-01-02T16:15:13Z","msg":"Unable to get configMap containing custom monitoring queries","controller":"cluster","controllerGroup":"postgresql.cnpg.io","controllerKind":"Cluster","Cluster":{"name":"cluster-with-metrics","namespace":"default"},"namespace":"default","name":"cluster-with-metrics","reconcileID":"53b16918-674a-4f72-b6ea-d7d4f5f4b098","uuid":"1bed7947-a98a-11ee-83b9-c633e1f4d6de","logging_pod":"cluster-with-metrics-1","reference":{"name":"example-monitoring","key":"custom-queries"},"error":"configmaps \"example-monitoring\" is forbidden: User \"system:serviceaccount:default:cluster-with-metrics\" cannot get resource \"configmaps\" in API group \"\" in the namespace \"default\""}
LOGS for cluster-with-metrics-2 :
{"level":"info","ts":"2024-01-02T16:15:13Z","msg":"Unable to get configMap containing custom monitoring queries","controller":"cluster","controllerGroup":"postgresql.cnpg.io","controllerKind":"Cluster","Cluster":{"name":"cluster-with-metrics","namespace":"default"},"namespace":"default","name":"cluster-with-metrics","reconcileID":"3e46260c-e1b0-4d9c-97d2-2d56302b7d4e","uuid":"1bedc557-a98a-11ee-a58c-866348f5044e","logging_pod":"cluster-with-metrics-2","reference":{"name":"example-monitoring","key":"custom-queries"},"error":"configmaps \"example-monitoring\" is forbidden: User \"system:serviceaccount:default:cluster-with-metrics\" cannot get resource \"configmaps\" in API group \"\" in the namespace \"default\""}
LOGS for cluster-with-metrics-3 :
{"level":"info","ts":"2024-01-02T16:15:13Z","msg":"Unable to get configMap containing custom monitoring queries","controller":"cluster","controllerGroup":"postgresql.cnpg.io","controllerKind":"Cluster","Cluster":{"name":"cluster-with-metrics","namespace":"default"},"namespace":"default","name":"cluster-with-metrics","reconcileID":"78862135-086e-4347-9094-0b9b549e8e4e","uuid":"1bef6db6-a98a-11ee-bf86-46cf2005095a","logging_pod":"cluster-with-metrics-3","reference":{"name":"example-monitoring","key":"custom-queries"},"error":"configmaps \"example-monitoring\" is forbidden: User \"system:serviceaccount:default:cluster-with-metrics\" cannot get resource \"configmaps\" in API group \"\" in the namespace \"default\""}
```
