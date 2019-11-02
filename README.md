# kubernetes-learning

## kubernetes cluster Setup scenario

### Add user

1 adduser bing 

2 passwd bing 

3 chmod -v u+w /etc/sudoers 

4 vim /etc/sudoers 

5 
```java
root ALL=(ALL) ALL
bing ALL=(ALL) ALL 
```
6 chmod -v u-w /etc/sudoers

7 su bing


### Install docker in centos 7
1 
```java
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

2 
```java
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

3 
```java
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

4 sudo yum install docker-ce docker-ce-cli containerd.io

5 sudo systemctl enable docker

6 sudo systemctl start docker

7 sudo docker run hello-world

### Install k8s
1
```java
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable --now kubelet
```


2 creat master
kubeadm init --apiserver-advertise-address master-ip --pod-network-cidr=10.224.0.0/16

3
```java
su bing
mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
echo "source <(kubectl completion bash)" >> ~/.bashrc
```
4 wget --no-check-certificate https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

5 kubectl apply -f kube-flannel.yml

6 join worker

kubeadm join 172.31.63.236:6443 --token r4iccp.0bikb7szse309o6d \
    --discovery-token-ca-cert-hash sha256:ab72ad8fb2aa9e999522c401ee3bdd543555813515230b932b0c8bf3e8d31689
    
7 kubectl get nodes
