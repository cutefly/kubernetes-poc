# Kubernetes cluster 설치

```
# OS 설치(on vmware)
Ubuntu 20.04(k8s-master1, k8s-worker1, k8s-worker2)

# timezone 설정
$ sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```

### Installing runtime

```
# Install docker
https://kubernetes.io/docs/setup/production-environment/container-runtimes/

$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

$ sudo usermod -aG docker $USER

# Set up the Docker daemon(Cgroup drivers)
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

mkdir -p /etc/systemd/system/docker.service.d

# Restart Docker
systemctl daemon-reload
systemctl restart docker
```

### Installing kubeadm, kubelet and kubectl

```
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

$ sudo apt-get update && sudo apt-get install -y apt-transport-https curl
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
$ sudo apt-get update
$ sudo apt-get install -y kubelet kubeadm kubectl
$ sudo apt-mark hold kubelet kubeadm kubectl

# Auto completion
https://kubernetes.io/ko/docs/tasks/tools/install-kubectl/
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

### Create cluster

```
# Disable swap
$ sudo swapoff -a

$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16
W0730 19:03:12.698283  222366 configset.go:202] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]
[init] Using Kubernetes version: v1.18.6
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [k8s-master1 kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 172.16.3.204]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [k8s-master1 localhost] and IPs [172.16.3.204 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [k8s-master1 localhost] and IPs [172.16.3.204 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
W0730 19:03:25.257842  222366 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[control-plane] Creating static Pod manifest for "kube-scheduler"
W0730 19:03:25.261074  222366 manifests.go:225] the default kube-apiserver authorization-mode is "Node,RBAC"; using "Node,RBAC"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 37.503703 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.18" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node k8s-master1 as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node k8s-master1 as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: kkakl1.q85mgo6ri3v1sh97
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[kubelet-check] Initial timeout of 40s passed.
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.16.3.204:6443 --token kkakl1.q85mgo6ri3v1sh97 \
    --discovery-token-ca-cert-hash sha256:c7f6e51d0b7d8e4ce909f2b52e8745be001be5ad88923da4b7f84bea7b88dd8e


# pods network
https://github.com/coreos/flannel#flannel
https://github.com/coreos/flannel/blob/master/Documentation/kubernetes.md

$ kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
$ kubectl get pods --all-namespaces
$ kubectl describe -n kube-system pod kube-flannel-ds-amd64-tncn6
```

Control plane node isolation

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```

# Join the cluster

```
# Disable swap
$ sudo swapoff -a

sudo kubeadm join 172.16.3.204:6443 --token kkakl1.q85mgo6ri3v1sh97 \
    --discovery-token-ca-cert-hash sha256:c7f6e51d0b7d8e4ce909f2b52e8745be001be5ad88923da4b7f84bea7b88dd8e
```

# Install dashboard

```
https://github.com/kubernetes/dashboard#kubernetes-dashboard

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml
$ kubectl proxy
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

# 인증추가
$ kubectl apply -f kubernetes/dashboard-adminuser.yaml
$ kubectl apply -f /dashboard-rolebinding.yaml

# generate token
$ kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

# Basic logging in Kubernetes

```
https://kubernetes.io/docs/concepts/cluster-administration/logging/

$ kubectl apply -f https://k8s.io/examples/debug/counter-pod.yaml
```

# Ingress controller 설치(nginx)

```
https://github.com/kubernetes/ingress-nginx

Bare-metal
Using NodePort:
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.35.0/deploy/static/provider/baremetal/deploy.yaml

Verify installation
$ kubectl get pods -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx

NordPort 확인
$ kubectl get svc -n ingress-nginx
```

# Namespace 생성

```
$ kubectl create namespace kpcard-development
$ kubectl create namespace kpcard-production
```

# NodePort 테스트

```
NodePort : 31290
http://tnginx.k8s.local:31290/
http://thello.k8s.local:31290/
http://tcustom.k8s.local:31290/
```