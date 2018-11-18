### Cluster setup

Install docker, kubectl, kubelet and kubeadm on hosts machines:

```sh
apt udpate
curl -fsSL https://get.docker.com | bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list
apt update && apt install -y kubelet kubeadm kubectl
```

Init kubernetes cluster on master host:

```sh
echo "source <(kubectl completion bash)" >> ~/.bashrc
kubeadm init --apiserver-advertise-address=$(hostname -i) --ignore-preflight-errors=SystemVerification
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
```
Note: ignore system verification means bypassing kubernetes warning about using the newest version of docker

Join cluster on other hosts:

```sh
kubeadm join <MASTER_IP_ADDRESS>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash <HASH> --ignore-preflight-errors=SystemVerification
```

Init network addon (wavenet) on master host:

```sh
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
```

### Deployment 

Create kubernetes Deployment from configuration file:

```sh
kubectl create -f deploy.yml
```

List created deployments:

```sh
kubectl get deployments
```

Create a Service to load balance the cluster requests:

```sh
kubectl create -f service.yml
```

Create a ConfigMap volume (will be used later with prometheus)

```sh
kubectl create configmap prometheus-conf prometheus.yml
```
