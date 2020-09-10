# For Kubeflow namespace:

Set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=loadbalancer-mad-mex-0.5.0_0.1.0-local-pv
MAD_MEX_STORAGE=local-storageclass
MAD_MEX_PV=local-pv
MAD_MEX_PVC=local-pvc
MAD_MEX_JUPYTERLAB_SERVICE=jupyterlab-mad-mex-0.5.0_0.1.0-local-pv
MAD_MEX_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/minikube_sipecam/master/deployments/MAD_Mex/
```

Download and modify with ip of node for:

```
wget $MAD_MEX_URL/local_pv/$MAD_MEX_PV.yaml
wget $MAD_MEX_URL/local_pv/$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

Create storage:

```
kubectl create -f $MAD_MEX_URL/local_pv/$MAD_MEX_STORAGE.yaml
kubectl create -f $MAD_MEX_URL/local_pv/$MAD_MEX_PV.yaml
kubectl create -f $MAD_MEX_URL/local_pv/$MAD_MEX_PVC.yaml
```

Create service:

```
kubectl create -f $MAD_MEX_URL/local_pv/$MAD_MEX_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MAD_MEX_URL/local_pv/$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

To check set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=$(echo $MAD_MEX_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MAD_MEX_JUPYTERLAB_SERVICE=$(echo $MAD_MEX_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MAD_MEX_LOAD_BALANCER_SERVICE
kubectl describe storageclass -n kubeflow $MAD_MEX_STORAGE
kubectl describe pv -n kubeflow $MAD_MEX_PV
kubectl describe pvc -n kubeflow $MAD_MEX_PVC
kubectl describe deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE
```

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $MAD_MEX_LOAD_BALANCER_SERVICE
kubectl delete pvc -n kubeflow $MAD_MEX_PVC
kubectl delete pv -n kubeflow $MAD_MEX_PV
kubectl delete storageclass -n kubeflow $MAD_MEX_STORAGE
kubectl delete deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE
```
