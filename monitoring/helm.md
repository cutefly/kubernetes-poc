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


### kube-prometheus-stack

> https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack


```
$ kubectl create namespace monitoring
$ helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack --namespace monitoring

$ helm uninstall kube-prometheus-stack --namespace monitoring
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com
```

