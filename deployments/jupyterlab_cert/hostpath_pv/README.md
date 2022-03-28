
Set:

```
LOAD_BALANCER_SERVICE=loadbalancer-0.6.1-hostpath-pv
HOSTPATH_PV=hostpath-pv
HOSTPATH_PVC=hostpath-pvc
JUPYTERLAB_SERVICE_HOSTPATH_PV=jupyterlab-cert-0.6.1-hostpath-pv
URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/deployments/jupyterlab_cert/
```

Next lines are not necessary but help to modify services:

```
wget $URL/hostpath_pv/$LOAD_BALANCER_SERVICE.yaml
wget $URL/hostpath_pv/$HOSTPATH_PV.yaml
wget $URL/hostpath_pv/$HOSTPATH_PVC.yaml
wget $URL/hostpath_pv/$JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create service:

```
kubectl create -f $URL/hostpath_pv/$LOAD_BALANCER_SERVICE.yaml
```

Create storage:

```
kubectl create -f $URL/hostpath_pv/$HOSTPATH_PV.yaml
kubectl create -f $URL/hostpath_pv/$HOSTPATH_PVC.yaml
```

Create deployment:

```
kubectl create -f $URL/hostpath_pv/$JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```
