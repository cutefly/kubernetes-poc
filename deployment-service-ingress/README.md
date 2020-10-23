# Kubernetes Testing

```
0. Add hosts first(in /etc/hosts)
1. Create namespace
2. Apply deployment
3. Expose service
4. Configure ingress
```

### Add hosts

```
in /etc/hosts(C:/Windows/System32/drivers/etc/hosts)

# for development
172.16.3.191    tnginx.k8s.local
172.16.3.191    thello.k8s.local
172.16.3.191    tcustom.k8s.local

# for production
172.16.3.191    custom.k8s.local
```

### Create namespace

```
$ kubectl create namespace kpcard-development
$ kubectl create namespace kpcard-production
```

### Apply deployment and service

```
$ kubectl apply -f nginx-deploy-svc.yaml
$ kubectl apply -f hello-deploy-svc.yaml
$ kubectl apply -f custrom-deploy-svc.yaml
```

### Configure ingress

```
$ kubectl apply -f nginx-ingress.yaml
```

* development namespace

```
nginx: http://tnginx.k8s.local:32190/
hello: http://thello.k8s.local:32190/
custom: http://tcustom.k8s.local:32190/
```

* production namespace

```
custom: http://custom.k8s.local:32190/
```

## 프라이빗 레지스트리에서 이미지 받아오기

> https://kubernetes.io/ko/docs/tasks/configure-pod-container/pull-image-private-registry/

```
# Docker registry secrets 생성
$ kubectl create secret docker-registry kpcard-registry-cred --docker-server='docker.kpcard.co.kr' --docker-username='deploy' --docker-password='********' --docker-email='deploy@kpcard.co.kr'

# docker-registry-secrets.yaml
kind: Secret
apiVersion: v1
metadata:
  name: kpcard-registry-cred
  namespace: default
data:
  .dockerconfigjson: >-
    eyJhdXRocyI6eyJkb2NrZXIua3BjYXJkLmNvLmtyIjp7InVzZXJuYW1lIjoiZGVwbG95IiwicGFzc3dvcmQiOiJkZXBsb3kyMzM9KiIsImVtYWlsIjoiZGVwbG95QGtwY2FyZC5jby5rciIsImF1dGgiOiJaR1Z3Ykc5NU9tUmxjR3h2ZVRJek16MHEifX19
type: kubernetes.io/dockerconfigjson

# pod, deployment 등 생성 시 secrets 명시
spec:
  containers:
  - name: kpcard-homepage
    image: docker.kpcard.co.kr/kr.co.kpcard/kpcard-homepage:latest
  imagePullSecrets:
  - name: kpcard-registry-cred  # secrets 명시
```
