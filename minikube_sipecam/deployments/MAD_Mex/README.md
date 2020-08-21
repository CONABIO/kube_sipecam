# For Kubeflow namespace:

Set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=kale-service-kubeflow_0.5.0_0.1.0
MADMEX_KALE_JUPYTERLAB_SERVICE=kale-jupyterlab-kubeflow_0.1.0_1.8.3_0.5.0
MADMEX_KALE_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/MAD_Mex/
```

Next lines are not necessary but help to modify services:

```
wget $MADMEX_KALE_URL$MADMEX_KALE_LOAD_BALANCER_SERVICE.yaml
wget $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE.yaml
```

Create service:

```
kubectl create -f $MADMEX_KALE_URL/$MADMEX_KALE_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MADMEX_KALE_URL$MADMEX_KALE_JUPYTERLAB_SERVICE.yaml
```

To check set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=$(echo $MADMEX_KALE_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MADMEX_KALE_JUPYTERLAB_SERVICE=$(echo $MADMEX_KALE_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl describe deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE
```

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl delete deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE
```
