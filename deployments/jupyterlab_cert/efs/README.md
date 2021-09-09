
Set:

```
LOAD_BALANCER_SERVICE=loadbalancer-0.6.1-efs
JUPYTERLAB_SERVICE_HOSTPATH_PV=jupyterlab-cert-0.6.1-efs
URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/deployments/jupyterlab_cert/
```

Next lines are not necessary but help to modify services:

```
wget $URL/efs$LOAD_BALANCER_SERVICE.yaml
wget $URL/efs$JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create service:

```
kubectl create -f $URL/efs/$LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $URL/efs/$JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```
