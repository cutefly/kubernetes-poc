# CIFS를 이용한 Volume mount

```
NFS가 불가한 경우 CIFS로 마운트 고려
k8s에서 기본제공하는 옵션이 아닌 관계로 별도로 플러그인 설치 및 마운트

Node에 마운트한 후 hostPath로 사용하거나 hostPath를 이용하여 Persitent Volume 생성
```