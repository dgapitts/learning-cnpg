## Expose pg services


Reviewing the cnpg document [expose_pg_services](https://cloudnative-pg.io/documentation/1.18/expose_pg_services/):

> We assume that:
- the NGINX Ingress controller has been deployed and works correctly
- it is possible to create a service of type LoadBalancer in your cluster

Googling "ingress-nginx helm setup" I found [this gcore.com page](https://gcore.com/docs/cloud/kubernetes/networking/install-and-set-up-the-nginx-ingress-controller):

```
kubectl create namespace ingress-nginx
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update;
helm install ingress-nginx ingress-nginx/ingress-nginx  --namespace ingress --set controller.ingressClassResource.name=nginx
kubectl -n ingress-nginx get svc
```

Then switching back to the cnpg documentation

```
 kubectl apply -f cluster-expose-service.yaml
```

but I still don't have an external IP address
```
 $ kubectl get svc/ingress-nginx -n ingress-nginx
NAME            TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                                     AGE
ingress-nginx   LoadBalancer   10.96.134.12   <pending>     80:30953/TCP,443:32355/TCP,5432:32074/TCP   12h
```

and unfortunately googling "kind loadbalancer no external ip"

> kind does not support type=LoadBalancer currently, load balancers are cloud provider specific. see #99 for some discussion around this.
https://github.com/kubernetes-sigs/kind/issues/411

