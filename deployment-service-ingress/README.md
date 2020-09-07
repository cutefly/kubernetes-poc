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