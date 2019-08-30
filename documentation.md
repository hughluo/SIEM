# This is doc for SIEM on GKE

## Pre


## Install Helm
```
curl -LO https://git.io/get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
```

## Init Helm & Add Tiller service account
File neeed:
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
Intall through helm, also set service type to `LoadBalancer` in oder to expose to outside of the cluster.
```
helm install --name kibana elastic/kibana --set service.type=LoadBalancer
```

## Install Auditbit

```
docker run \
  --cap-add="AUDIT_CONTROL" \
  --cap-add="AUDIT_READ" \
  docker.elastic.co/beats/auditbeat:7.3.1 \
  setup -E setup.kibana.host=10.12.10.64:5601 \
  -E output.elasticsearch.hosts=["10.12.0.35:9200"]
```