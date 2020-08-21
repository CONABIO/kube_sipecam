# For Kubeflow namespace:

Set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=kale-service-kubeflow-0.5.0_0.1.0-local-pv
MADMEX_KALE_STORAGE=local-storageclass
MADMEX_KALE_PV=local-pv
MADMEX_KALE_PVC=local-pvc
MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV=kale-jupyterlab-kubeflow-0.5.0_0.1.0-local-pv
MADMEX_KALE_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/minikube_sipecam/master/deployments/MAD_Mex/
```

Download and modify with ip of node for:

```
wget $MADMEX_KALE_URL/local_pv/$MADMEX_KALE_PV.yaml
wget $MADMEX_KALE_URL/local_pv/$MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV.yaml
```

Create storage:

```
kubectl create -f $MADMEX_KALE_URL/local_pv/$MADMEX_KALE_STORAGE.yaml
kubectl create -f $MADMEX_KALE_PV.yaml
kubectl create -f $MADMEX_KALE_URL/local_pv/$MADMEX_KALE_PVC.yaml
```

Create service:

```
kubectl create -f $MADMEX_KALE_URL/local_pv/$MADMEX_KALE_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV.yaml
```

To check set:

```
MADMEX_KALE_LOAD_BALANCER_SERVICE=$(echo $MADMEX_KALE_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV=$(echo $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl describe storageclass -n kubeflow $MADMEX_KALE_STORAGE
kubectl describe pv -n kubeflow $MADMEX_KALE_PV
kubectl describe pvc -n kubeflow $MADMEX_KALE_PVC
kubectl describe deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $MADMEX_KALE_LOAD_BALANCER_SERVICE
kubectl delete pvc -n kubeflow $MADMEX_KALE_PVC
kubectl delete pv -n kubeflow $MADMEX_KALE_PV
kubectl delete storageclass -n kubeflow $MADMEX_KALE_STORAGE
kubectl delete deployment -n kubeflow $MADMEX_KALE_JUPYTERLAB_SERVICE_LOCAL_PV
```
