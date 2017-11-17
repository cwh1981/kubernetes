# kubernetes

[master]
centos 7 : 
1. Disable SELinux & setup firewall rules
- sudo systemctl disable firewalld
- sudo systemctl stop firewalld

2. Edit /etc/hosts
=> 192.168.1.30 k8s-master
192.168.1.40 worker-node1
192.168.1.50 worker-node2

3. Edit yum repo(/etc/yum.repos.d/kubernetes.repo)
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
       https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg

4. docker install
- sudo yum install kubeadm docker -y

5. Start and enable kubectl and docker service
[root@k8s-master ~]# systemctl restart docker && systemctl enable docker
[root@k8s-master ~]# systemctl  restart kubelet && systemctl enable kubelet

6. swap off
- sudo swapoff -a

7. Initialize Kubernetes Master with ‘kubeadm init’
- sudo kubeadm init --apiserver-advertise-address=xx.xxx.xxx.xxx
  - if it has a problem.
    - netstat -lnp | grep "duplicated port"
    - kill port
8. Execute the beneath commands to use the cluster as root user
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

9. kubectl get nodes
10. kubectl get pods --all-namespaces
11. 

[참조]
- https://www.linuxtechi.com/install-kubernetes-1-7-centos7-rhel7/
- 

[slave]
kubeadm join --token 5bcd73.aa70895034d49003 10.176.93.38:6443 --discovery-token-ca-cert-hash sha256:04e3eeb0bcde2356f71fa2c5c2f7db0ec7f08864a8a794aa92f9745b9aa28ea6
