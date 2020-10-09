# Persitent Volume(NFS)

> https://www.linuxtechi.com/configure-nfs-persistent-volume-kubernetes/

### Persitent Volume 생성

```
# pv 생성(persitent volume)
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  storageClassName: nfs
  nfs:
    path: /mnt/data
    server: 172.16.3.204

# pvc 생성(persitent volume claim)
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  volumeName: nfs-pv
  storageClassName: nfs
  volumeMode: Filesystem

# pvc에서 volumeName으로 pv명(nfs-pv)을 지정해야 함.
```

### Persitent Volume 적용

```
# volumes에서 PVC의 이름(nfs-pvc:nginx-pv-storage)을 정의
# volumeMounts에서 container의 경로(/usr/share/nginx/html)를 pvc와 매핑된 volume명(nginx-pv-storage)으로 Mount
# 
kind: Deployment
spec:
  template:
    spec:
      containers:
      - name: my-nginx
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: nginx-pv-storage
      volumes:
      - name: nginx-pv-storage
        persistentVolumeClaim:
          claimName: nfs-pvc
```
