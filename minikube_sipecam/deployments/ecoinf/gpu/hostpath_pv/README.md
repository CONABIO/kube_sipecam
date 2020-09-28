# For Kubeflow namespace:

Set:

```
ECOINF_KUBEFLOW_NAMESPACE=kubeflow-namespace
ECOINF_LOAD_BALANCER_SERVICE_GPU=loadbalancer-ecoinf-gpu-0.5.0
ECOINF_PV=hostpath-pv
ECOINF_PVC=hostpath-pvc
ECOINF_JUPYTERLAB_SERVICE_GPU=jupyterlab-ecoinf-gpu-0.5.0
ECOINF_URL_GPU=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/ecoinf/gpu
```

Next lines are not necessary but help to modify services:

```
wget $ECOINF_URL_GPU/hostpath_pv/$ECOINF_LOAD_BALANCER_SERVICE_GPU.yaml
wget $ECOINF_URL/hostpath_pv/$ECOINF_PV.yaml
wget $ECOINF_URL/hostpath_pv/$ECOINF_PVC.yaml
wget $ECOINF_URL_GPU/hostpath_pv/$ECOINF_KUBEFLOW_NAMESPACE.yaml
wget $ECOINF_URL_GPU/hostpath_pv/$ECOINF_JUPYTERLAB_SERVICE_GPU.yaml
```

Create kubeflow namespace:

```
kubectl create -f $ECOINF_URL_GPU/hostpath_pv/$ECOINF_KUBEFLOW_NAMESPACE.yaml
```

Create storage:

```
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_PV.yaml
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_PVC.yaml
```


Create service:

```
kubectl create -f $ECOINF_URL_GPU/hostpath_pv/$ECOINF_LOAD_BALANCER_SERVICE_GPU.yaml
```

Create deployment:

```
kubectl create -f $ECOINF_URL_GPU/hostpath_pv/$ECOINF_JUPYTERLAB_SERVICE_GPU.yaml
```

To check set:

```
ECOINF_LOAD_BALANCER_SERVICE_GPU=$(echo $ECOINF_LOAD_BALANCER_SERVICE_GPU|sed -n 's/\./-/g;s/_/-/g;p')
ECOINF_JUPYTERLAB_SERVICE_GPU=$(echo $ECOINF_JUPYTERLAB_SERVICE_GPU|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $ECOINF_LOAD_BALANCER_SERVICE_GPU
kubectl describe pv -n kubeflow $ECOINF_PV
kubectl describe pvc -n kubeflow $ECOINF_PVC
kubectl describe deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE_GPU
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE_GPU --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $ECOINF_LOAD_BALANCER_SERVICE_GPU
kubectl delete pvc -n kubeflow $ECOINF_PVC
kubectl delete pv -n kubeflow $ECOINF_PV
kubectl delete deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE_GPU
```
