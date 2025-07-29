# Kind

# 前提：Docker 已被安装

Ubuntu 安装 docker：

```shellscript
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker info
```

# 安装 kind

```shellscript
sudo curl -o /usr/local/bin/kind -L https://ghproxy.net/https://github.com/kubernetes-sigs/kind/releases/download/v0.29.0/kind-linux-amd64
sudo chmod +x /usr/local/bin/kind
kind version

# 使用 kind 创建集群
tee ~/.kind-config.yaml <<-EOF
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  image: swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/kindest/node:v1.29.10
- role: worker
  image: swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/kindest/node:v1.29.10
- role: worker
  image: swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/kindest/node:v1.29.10
EOF
kind create cluster --config ~/.kind-config.yaml
```

```shellscript
# 安装 kubectl
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates gnupg curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
kubectl version --client

# 安装命令自动补全
sudo apt install -y bash-completion
echo "source <(kubectl completion bash)" >> ~/.bashrc
source <(kubectl completion bash)
```

```shellscript
kubectl get nodes
kubectl get pods -A
kubectl create deployment myapp --image=swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/wangyanglinux/myapp:v1.0 --replicas=3
kubectl get pods
```
