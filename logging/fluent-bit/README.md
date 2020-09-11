# Fluent Bit

### Installation Fluent Bit

> https://docs.fluentbit.io/manual/installation/kubernetes

```
# Namespace
$ kubectl create namespace logging

# Role Binding
$ kubectl create -f https://raw.githubusercontent.com/fluent/fluent-bit-kubernetes-logging/master/fluent-bit-service-account.yaml
$ kubectl create -f https://raw.githubusercontent.com/fluent/fluent-bit-kubernetes-logging/master/fluent-bit-role.yaml
$ kubectl create -f https://raw.githubusercontent.com/fluent/fluent-bit-kubernetes-logging/master/fluent-bit-role-binding.yaml

# Config Map
$ kubectl create -f https://raw.githubusercontent.com/fluent/fluent-bit-kubernetes-logging/master/output/elasticsearch/fluent-bit-configmap.yaml
```

### Fluent Bit to Elasticsearch

```
$ kubectl apply -f https://raw.githubusercontent.com/fluent/fluent-bit-kubernetes-logging/master/output/elasticsearch/fluent-bit-ds.yaml

# elasticsearch 서버 변경
$ kubectl apply -f logging/fluent-bit-ds.yaml
```
