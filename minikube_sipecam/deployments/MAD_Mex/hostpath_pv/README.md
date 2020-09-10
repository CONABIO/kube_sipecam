# For Kubeflow namespace:

Set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=loadbalancer-mad-mex-0.1.0_1.7.0_0.5.0-hostpath-pv
MAD_MEX_PV=hostpath-pv
MAD_MEX_PVC=hostpath-pvc
MAD_MEX_JUPYTERLAB_SERVICE=jupyterlab-mad-mex-0.1.0_1.7.0_0.5.0-hostpath-pv
MAD_MEX_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/MAD_Mex/
```

Next lines are not necessary but help to modify services:

```
wget $MAD_MEX_URL$MAD_MEX_LOAD_BALANCER_SERVICE.yaml
wget $MAD_MEX_URL/hostpath_pv/$MAD_MEX_PV.yaml
wget $MAD_MEX_URL/hostpath_pv/$MAD_MEX_PVC.yaml
wget $MAD_MEX_URL/hostpath_pv/$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

Create storage:

```
kubectl create -f $MAD_MEX_URL/hostpath_pv/$MAD_MEX_PV.yaml
kubectl create -f $MAD_MEX_URL/hostpath_pv/$MAD_MEX_PVC.yaml
```

Create service:

```
kubectl create -f $MAD_MEX_URL/hostpath_pv/$MAD_MEX_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MAD_MEX_URL/hostpath_pv/$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

To check set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=$(echo $MAD_MEX_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MAD_MEX_JUPYTERLAB_SERVICE=$(echo $MAD_MEX_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MAD_MEX_LOAD_BALANCER_SERVICE
kubectl describe pv -n kubeflow $MAD_MEX_PV
kubectl describe pvc -n kubeflow $MAD_MEX_PVC
kubectl describe deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE
```

Describe all pods:

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
kubectl delete deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE 
```
