# AWS

## Instance

In AWS we can select ami: `k8s-1.16-debian-buster-amd64-hvm-ebs-2020-04-27 - ami-0ab39819e336a3f3f` and instance `m5.2xlarge` with `50` gb of disk.

Use next bash script for user data, this will start minikube using `none` driver:

```
#!/bin/bash
##variables:
region=us-west-2
user=admin
name_instance=minikube
shared_volume=/shared_volume
##System update
export DEBIAN_FRONTEND=noninteractive
apt-get update -yq
##Install awscli
apt-get install -y python3-pip && pip3 install --upgrade pip
pip3 install awscli --upgrade
##Tag instance
INSTANCE_ID=$(curl -s http://instance-data/latest/meta-data/instance-id)
PUBLIC_IP=$(curl -s http://instance-data/latest/meta-data/public-ipv4)
aws ec2 create-tags --resources $INSTANCE_ID --tag Key=Name,Value=$name_instance-$PUBLIC_IP --region=$region
#check if locales are ok with next lines:
echo "export LC_ALL=C.UTF-8" >> /root/.profile
echo "export LANG=C.UTF-8" >> /root/.profile
echo "export mount_point=$shared_volume" >> /root/.profile
systemctl start docker
usermod -aG docker $user
newgrp docker
#Create shared volume
mkdir $shared_volume
#kubectl installation
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
#bash completion, needs to exit and enter again to take effect
#echo "source <(kubectl completion bash)" >> /root/.bashrc
#apt-get install -y bash-completion
#minikube installation
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
cp minikube /usr/local/bin/
install minikube /usr/local/bin/
apt-get install conntrack -y
#kubeflow installation
cd /root && wget https://github.com/kubeflow/kfctl/releases/download/v1.0.2/kfctl_v1.0.2-0-ga476281_linux.tar.gz
tar -xvf kfctl_v1.0.2-0-ga476281_linux.tar.gz
echo "export PATH=$PATH:$(pwd)" >> /root/.profile
# Set KF_NAME to the name of your Kubeflow deployment. This also becomes the
# name of the directory containing your configuration.
# For example, your deployment name can be 'my-kubeflow' or 'kf-test'.
echo "export KF_NAME=kf-test" >> ~/.profile
echo "export BASE_DIR=/opt" >> ~/.profile
source ~/.profile
echo "export KF_DIR=${BASE_DIR}/${KF_NAME}" >> ~/.profile
```
Comment: a warning came out using last script:
```
WARNING! Your environment specifies an invalid locale.
 The unknown environment variables are:
   LC_CTYPE=UTF-8 LC_ALL=
 This can affect your user experience significantly, including the
 ability to manage packages. You may install the locales by running:

 sudo dpkg-reconfigure locales

 and select the missing language. Alternatively, you can install the
 locales-all package:

 sudo apt-get install locales-all
```

Check installation in AWS instance with: `tail -n 15  /var/log/cloud-init-output.log`.

Change to root with `sudo su`

Ssh to instance, then:

```
CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.0-branch/kfdef/kfctl_k8s_istio.v1.0.2.yaml"
source ~/.profile
chmod gou+wrx -R /opt/
mkdir -p ${KF_DIR}
#minikube start
cd /root && minikube start --driver=none
#kubeflow start
cd ${KF_DIR} && kfctl apply -V -f ${CONFIG_URI}
```

Check pods and status with:

`kubectl get pods -n kubeflow`

```
#all running except:
spark-operatorcrd-cleanup-2p7x2                                0/2     Completed   0          7m6s
```

To access kubeflow UI set:

```
export INGRESS_HOST=$(minikube ip)
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
echo $INGRESS_PORT
```

And go to:

```
http://<ipv4 of ec2 instance>:$INGRESS_PORT
```

### Deployments

Select one of the deployments inside [minikube_sipecam/deployments](../deployments)


# Local machine

Next will use `docker` driver. See: https://minikube.sigs.k8s.io/docs/drivers/docker/

Install minikube. For this I will use:

https://kubernetes.io/docs/setup/learning-environment/minikube/

https://kubernetes.io/docs/tasks/tools/install-minikube/

https://minikube.sigs.k8s.io/docs/drivers/docker/ 



```
$curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube
$sudo cp minikube /usr/local/bin/
$sudo install minikube /usr/local/bin/

$minikube start --driver=docker --alsologtostderr -v=1

```

Check:

```

$minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured


$kubectl cluster-info
Kubernetes master is running at https://172.17.0.3:8443
KubeDNS is running at https://172.17.0.3:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```

Although this option with `docker` driver worked I couldn't have access via browser to example of hello-minikube https://kubernetes.io/docs/setup/learning-environment/minikube/

I tried:

https://minikube.sigs.k8s.io/docs/handbook/accessing/#loadbalancer-access
https://kubernetes.github.io/ingress-nginx/deploy/#minikube

https://github.com/kubernetes/minikube/issues/7879
https://github.com/eox-dev/minikube-wsl2

but no success.

For Virtualbox driver: https://minikube.sigs.k8s.io/docs/drivers/virtualbox/ needs hyperv to be turned on

check for example: 

https://www.virtualbox.org/wiki/Linux_Downloads

https://aws.amazon.com/es/blogs/compute/running-hyper-v-on-amazon-ec2-bare-metal-instances/

or if want to skip check

https://github.com/docker/machine/issues/4271

```
#check if this steps are correct because could be installed with a apt-get install virtualbox-6.1 after doing some add repositories check https://www.virtualbox.org/wiki/Linux_Downloads
wget https://download.virtualbox.org/virtualbox/6.1.12/virtualbox-6.1_6.1.12-139181~Debian~buster_amd64.deb
sudo dpkg -i virtualbox-6.1_6.1.12-139181~Debian~buster_amd64.deb
sudo apt --fix-broken install
minikube start --driver=virtualbox --alsologtostderr -v=7
```

See for kubeflow 

https://www.arrikto.com/minikf/#installation-guide

https://www.kubeflow.org/docs/started/workstation/getting-started-minikf/

(uses VirtualBox and Vagrant)


### Dashboard

Next commands were used and most of the `minikube` cmds didn't work. Maybe one of them could be used for `minikube dashboard` see https://kubernetes.io/docs/setup/learning-environment/minikube/#examples or https://gist.github.com/F21/08bfc2e3592bed1e931ec40b8d2ab6f5

```
following https://kubernetes.io/docs/setup/production-environment/container-runtimes/

sudo su

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

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system

# Restart Docker
systemctl daemon-reload
sudo systemctl enable docker.service
sudo systemctl enable kubelet.service

sudo systemctl start docker
sudo systemctl start kubelet.service


minikube start --vm-driver=none --extra-config=apiserver.authorization-mode=RBAC --extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf --extra-config kubeadm.ignore-preflight-errors=SystemVerification --extra-config=kubelet.cgroup-driver=systemd --alsologtostderr -v=1

sudo minikube start --vm-driver=none --extra-config=apiserver.authorization-mode=RBAC --extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf --extra-config kubeadm.ignore-preflight-errors=SystemVerification --alsologtostderr -v=1

--------



minikube start --vm-driver=docker --extra-config=apiserver.authorization-mode=RBAC --extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf --extra-config kubeadm.ignore-preflight-errors=SystemVerification --extra-config=kubelet.cgroup-driver=systemd --alsologtostderr -v=1

-----

minikube start --vm-driver=docker --extra-config=apiserver.authorization-mode=RBAC --extra-config=kubelet.resolv-conf=/run/systemd/resolve/resolv.conf --extra-config kubeadm.ignore-preflight-errors=SystemVerification --alsologtostderr -v=1

```

# References

Info regarding pv, pvc:

https://kubernetes.io/docs/concepts/storage/persistent-volumes/#class-1

https://kubernetes.io/docs/concepts/storage/storage-classes/

https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim

https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/



https://kubernetes.io/docs/concepts/storage/volumes/#local

https://kubernetes.io/docs/concepts/storage/storage-classes/#local


https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolume

Install kubectl. For this I will use: https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-linux

https://kubernetes.io/docs/tasks/tools/install-minikube/ to install minikube

For None driver: https://minikube.sigs.k8s.io/docs/drivers/none/

https://kubernetes.io/docs/setup/learning-environment/minikube/

(next wasn't successful)
Another option to install minikube following https://www.kubeflow.org/docs/started/workstation/minikube-linux/


**Also see that swap is disabled** see: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/ if not disabled I could use:

```
#turn off swap

sudo swapoff -a
sudo systemctl restart kubelet.service
```

Follow: https://www.kubeflow.org/docs/started/workstation/minikube-linux/

Check also: https://www.kubeflow.org/docs/started/workstation/getting-started-linux/


See for kubeflow 

https://www.arrikto.com/minikf/#installation-guide

https://www.kubeflow.org/docs/started/workstation/getting-started-minikf/

(uses VirtualBox and Vagrant)
