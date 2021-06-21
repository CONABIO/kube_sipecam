
Dummy line to trigger build

Set:

```
HSI_LOAD_BALANCER_SERVICE=loadbalancer-0.6.1-efs
HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV=jupyterlab-cert-0.6.1-efs
HSI_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/deployments/jupyterlab_cert/
```

Next lines are not necessary but help to modify services:

```
wget $HSI_URL/efs$HSI_LOAD_BALANCER_SERVICE.yaml
wget $HSI_URL/efs$HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create service:

```
kubectl create -f $HSI_URL/efs/$HSI_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $HSI_URL/efs/$HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```
