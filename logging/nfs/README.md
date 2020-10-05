# Persitent Volume(NFS)

> https://www.linuxtechi.com/configure-nfs-persistent-volume-kubernetes/

### Persitent Volume 생성

```
# pv 생성(persitent volume)
kind: PersistentVolume
spec:
  storageClassName: nfs
  nfs:
    path: /mnt/data
    server: 172.16.3.204

# pvc 생성(persitent volume claim)
kind: PersistentVolumeClaim
spec:
  storageClassName: nfs

# pv, pvc의 storageClassName이 동일해야 함.
```

### Persitent Volume 적용

```
# volumes에서 PVC의 이름(nginx-pv-storage)을 정의
# container의 volumeMount에 정의된 이름(nginx-pv-storage)으로 Mount
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
