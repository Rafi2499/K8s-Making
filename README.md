# K8s-Making

------------------------------------------------------------------------------------------------------------
# Common for Master and Slave in Ubuntu
# Switch to root user
sudo su
# Update Package Repositories and Install Dependencies
apt-get update && \
apt-get install -y apt-transport-https && \
apt install -y docker.io && \
systemctl start docker && \
systemctl enable docker
# Add Kubernetes Repository Key
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
# Add Kubernetes Repository
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list && \
apt-get update
# Install Kubernetes Components
apt-get install -y kubelet kubeadm kubectl kubernetes-cni

# And For The Master Continue here
# Initialize Kubernetes Master
sudo kubeadm init
# Set Up Kubeconfig for Current User
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
wget https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/k8s-old-manifests/kube-flannel-rbac.yml
kubectl apply -f kube-flannel-rbac.yml 
# There will me a message like this in while commanding **sudo kubeadm init** copy this and pste in Slave i.e., 
( root@ip-111-11-11-111:/home/ubuntu# kubeadm join 222.22.22.22:6443 --token 6nyc... --discovery-token-ca-cert-hash sha256:a7616c9...  )

------------------------------------------------------------------------------------------------------------------------
