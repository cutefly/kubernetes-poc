# Helm을 이용한 Monitoring 구축

> https://www.metricfire.com/blog/monitoring-kubernetes-tutorial-using-grafana-and-prometheus/

```
$ kubectl create ns monitoring
$ helm install prometheus-operator stable/prometheus-operator --namespace monitoring
```

```
# ingress controller 설정(grafana 예시)
# spec:
#   rules:
#   - host: grafana.k8s.local
#     http:
#       paths:
#       - path: /
#         backend:
#           serviceName: prometheus-operator-grafana
#           servicePort: 80

$ kubectl apply -f nginx-ingress.yaml

http://grafana.k8s.local/
Account : admin / prom-operator
```

### kube-prometheus-stack(final)

> https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

```
# namespace 생성
$ kubectl create namespace monitoring

# helm repository 지정
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo add stable https://kubernetes-charts.storage.googleapis.com/
$ helm repo update

# install
$ helm install --name kube-prometheus-stack prometheus-community/kube-prometheus-stack

# install(custom values)
$ helm install -f values.yaml kube-prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring

# uninstall
$ helm uninstall kube-prometheus-stack --namespace monitoring
kubectl delete secrets kube-prometheus-stack-admission -n monitoring
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com
```

```
# grafana.k8s.local ingress 추가
# install
$ kubectl apply -f ingress-monitoring.yaml

# uninstall
$ kubectl delete -f ingress-monitoring.yaml

http://grafana.k8s.local:32080/
```
