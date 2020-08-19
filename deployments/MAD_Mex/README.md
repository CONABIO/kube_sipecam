# For Kubeflow namespace:

Set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=kale-service-kubeflow_0.5.0_0.1.0
MADMEX_KALE_JUPYTERLAB_SERVICE=kale-jupyterlab-kubeflow_0.5.0_0.1.0
MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV=kale-jupyterlab-kubeflow_0.5.0_0.1.0-local-pv
MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV=kale-jupyterlab-kubeflow_0.5.0_0.1.0-hostpath-pv
MADMEX_KALE_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/deployments/MAD_Mex/
```

Next lines are not necessary but help to modify services:

```
wget $MADMEX_KALE_URL$MADMEX_KALE_LOAD_BALANCER_SERVICE.yaml
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE.yaml
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV.yaml
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create service:

```
kubectl create -f $MADMEX_KALE_URL/$MADMEX_KALE_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE.yaml
kubectl create -f $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV.yaml
kubectl create -f $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

To check set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=$(echo $MADMEX_KALE_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MADMEX_KALE_JUPYTERLAB_SERVICE=$(echo $MADMEX_KALE_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV=$(echo $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV|sed -n 's/\./-/g;s/_/-/g;p')
MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV=$(echo $MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl describe deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE
kubectl describe deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
kubectl describe deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV
```

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl delete deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE
kubectl delete deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
kubectl delete deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_HOSTPATH_PV 
```