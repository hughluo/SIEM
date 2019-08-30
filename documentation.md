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