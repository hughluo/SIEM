# SIEM on Go

## Install Helm
```
curl -LO https://git.io/get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

## Init Helm & Add Tiller service account
File needed:
+ k8s/helm/rbac-config.yaml
```
kubectl create -f rbac-config.yaml
helm init --service-account tiller --history-max 200
```
## Install Elasticsearch

Install through helm.
```
helm repo add elastic https://helm.elastic.co
helm install --name elasticsearch elastic/elasticsearch
```


## Install Kibana
Install through helm, also set service type to `LoadBalancer` in oder to expose to outside of the cluster.
```
helm install --name kibana elastic/kibana --set service.type=LoadBalancer
```

## Install Auditbeat

File needed:
+ auditbeat/auditbeat-kubernetes.yaml

In which, the host and port of Kibana in `ConfigMap` need to be modified to the allocated `CLUSTER-IP` and `PORT`.

Also the environment variable in the container that related to the host and port of Elasticsearch need to be modified to the allocated `CLUSTER-IP` and `PORT`. 

```
kubectl create -f auditbeat-kubernetes.yaml
```

## Finish
Now we can go visit the `EXTERNAL-IP` of Kibana to check whether it works.

