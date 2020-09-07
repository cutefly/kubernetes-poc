# Monitoring kubernetes 설치

> Referene: https://gruuuuu.github.io/cloud/monitoring-02/#

* Prometheus 배포

```
Namespace 생성
ClusterRole, ClusterRoleBinding 배포
ConfigMap 생성
Prometheus deployment 및 서비스 노출
DaemonSet에 node-exporter 구성 및 서비스 노출
```

```
$ kubectl create namespace monitoring

$ kubectl apply -f prometheus-cluster-role.yaml
$ kubectl apply -f prometheus-config-map.yaml
$ kubectl apply -f prometheus-deployment.yaml
$ kubectl apply -f prometheus-node-exporter.yaml
$ kubectl apply -f prometheus-svc.yaml
```

* Kube State Metrics 배포

```
ClusterRole, ClusterRoleBinding 배포
ClusterRole과 연동될 서비스어카운트를 생성
kube-state-metrics deployment 및 서비스 노출
```

```
$ kubectl apply -f kube-state-cluster-role.yaml
$ kubectl apply -f kube-state-svcaccount.yaml
$ kubectl apply -f kube-state-deployment.yaml
$ kubectl apply -f kube-state-svc.yaml
```

* Grafana 연동

```
Deployment 및 Service 노출
Dashboard 구성
```

```
$ kubectl apply -f grafana.yaml
```

### 나머지

```
grafana나 prometheus는 CluterIP(NodePort가 아닌)로 설정한 후 nginx-ingress를 통해 외부 접속 가능
grafana: http://grafana.k8s.local:31290/

# grafana plugin 설치
# grafana shell 얻기
$ kubectl exec -it -n monitoring grafana-64c967bd7-gkfnm -- bash
# plugin 설치
bash-5.0$ grafana-cli plugins install btplc-status-dot-panel
```