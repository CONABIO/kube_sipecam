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

`minikube status`

```
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

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