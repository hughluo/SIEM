# SIEM

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
Install through helm, do the following settings:

+ Enable dashboard
+ Set host of Kibana service
+ Disable output as file
+ Set the output to the Elasticsearch service need to be modified to the allocated <service_name:port>


```
helm install --name my-release --set config.setup.dashboards.enabled=true --set config.setup.kibana.host="kibana-kibana:5601" --set config.output.file.enabled=false --set config.output.elasticsearch.hosts="elasticsearch-master" stable/auditbeat 
```

## Finish
Now we can go visit the `EXTERNAL-IP` of Kibana to check whether it works.

