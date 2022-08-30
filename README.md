# KubernetesSetup

# Pre-reqs for Master and Worker (we are using centos 7.6)

-> uname -a

-> free -m or dmidecode

-> nproc

-> free -m   (also cat /etc/fstab) --> if you see anything with SWAP as FS just comment that line and run swapoff -a 

-> getenforce
   setenforce 0
   vi /etc/selinux/config     --> change enforcing to permissive and save the file
   getenforce

-> hostnamectl set-hostname thinknyxmaster  (remember just allow two special characters in names one in "." And one is "-")
-> hostnamectl set-hostname thinknyxworkerone

-> ip addr (find the private ips of both server)
-> vi /etc/hosts (on both server and add IP to name mapping for both server)
-> Once done ping worker from master and vice versa

-> Port are all open

# Letting iptables see bridged traffic

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf /n
net.bridge.bridge-nf-call-ip6tables = 1 /n
net.bridge.bridge-nf-call-iptables = 1 /n
EOF
sudo sysctl --system

# Installing CRI-O as a runtime

--> export VERSION=1.24
--> export OS=CentOS_7

--> echo $VERSION
--> echo $OS

curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable.repo https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/devel:kubic:libcontainers:stable.repo

curl -L -o /etc/yum.repos.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/devel:kubic:libcontainers:stable:cri-o:$VERSION.repo

yum repolist 

yum install cri-o

systemctl status crio
systemctl start crio
systemctl enable crio
systemctl status crio

crio --version

