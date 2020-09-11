# EFK on k8s

> https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes

```
Step1 — How to create a Namespace?
Step2 — How to Create the Elasticsearch StatefulSet?
Step3 — How to create Kibana deployments and services?
Step4 — How to Create the Fluentd DaemonSet in the cluster?
```

### Create namespace

```
$ kubectl create -f kube-log.yaml
```

### Create elasticsearch service and statefulset

```
# persistent volume claim을 어떻게 처리할지 검토.....
# voiume, volumeClaim, storageClass 간의 관계를 잘 모르겠음
# persitemtVolume, persitentVolmueClaim 생성 ing...
$ kubectl apply -f persistent-volume.yaml

$ kubectl create -f elasticsearch_svc.yaml
$ kubectl create -f elasticsearch_statefulset.yaml
```

### Creating the Kibana deployment and service

```
$ kubectl create -f kibana.yaml
$ kubectl rollout status deployment/kibana --namespace=kube-logging
$ kubectl port-forward kibana-6c9fb4b5b7-plbg2 5601:5601 --namespace=kube-logging
```

###

```
kubectl create -f fluentd.yaml
```
