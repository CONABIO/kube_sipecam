# For Kubeflow namespace:

Set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=kale-service-kubeflow_0.5.0_0.1.0
MADMEX_KALE_JUPYTERLAB_SERVICE=kale-jupyterlab-kubeflow_0.5.0_0.1.0
MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV=kale-jupyterlab-kubeflow_0.5.0_0.1.0-local-pv
MADMEX_KALE_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/deployments/MAD_Mex/
```

Next lines are not necessary but help to modify services:

```
wget $MADMEX_KALE_URL$MADMEX_KALE_LOAD_BALANCER_SERVICE
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```

Create service:

```
kubectl create -f $MADMEX_KALE_URL/$MADMEX_KALE_LOAD_BALANCER_SERVICE
```

Create deployment:

```
kubectl create -f $MADMEX_KALE_URL/$MADMEX_KALE_JUPYTERLAB_SERVICE
kubectl create -f $MADMEX_KALE_URL/$MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```

Check:

```
kubectl describe service $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl describe service $MADMEX_KALE_JUPYTERLAB_SERVICE
kubectl describe service $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```

```
kubectl describe pods
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment $MADMEX_KALE_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment $MADMEX_KALE_JUPYTERLAB_SERVICE --replicas=<0 or 1>
kubectl scale deployment $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV --replicas=<0 or 1>
```

Delete:

```
kubectl delete service $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl delete service $MADMEX_KALE_JUPYTERLAB_SERVICE
kubectl delete service $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```
