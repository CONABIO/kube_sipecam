# For Kubeflow namespace:

Set:

```
HSI_LOAD_BALANCER_SERVICE=loadbalancer-hsi-0.6.1-hostpath-pv
HSI_PV=hostpath-pv
HSI_PVC=hostpath-pvc
HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV=jupyterlab-hsi-0.6.1-hostpath-pv
HSI_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/hsi/
```

Next lines are not necessary but help to modify services:

```
wget $HSI_URL/hostpath_pv/$HSI_LOAD_BALANCER_SERVICE.yaml
wget $HSI_URL/hostpath_pv/$HSI_PV.yaml
wget $HSI_URL/hostpath_pv/$HSI_PVC.yaml
wget $HSI_URL/hostpath_pv/$HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create storage:

```
kubectl create -f $HSI_URL/hostpath_pv/$HSI_PV.yaml
kubectl create -f $HSI_URL/hostpath_pv/$HSI_PVC.yaml
```

Create service:

```
kubectl create -f $HSI_URL/hostpath_pv/$HSI_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $HSI_URL/hostpath_pv/$HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

To check set:

```
HSI_LOAD_BALANCER_SERVICE=$(echo $HSI_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV=$(echo $HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $HSI_LOAD_BALANCER_SERVICE
kubectl describe pv -n kubeflow $HSI_PV
kubectl describe pvc -n kubeflow $HSI_PVC
kubectl describe deployment -n kubeflow $HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $HSI_LOAD_BALANCER_SERVICE
kubectl delete pvc -n kubeflow $HSI_PVC
kubectl delete pv -n kubeflow $HSI_PV
kubectl delete deployment -n kubeflow $HSI_JUPYTERLAB_SERVICE_HOSTPATH_PV
```
