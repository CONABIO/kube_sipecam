# For Kubeflow namespace:

Set:

```
TORCH_LOAD_BALANCER_SERVICE_CPU=loadbalancer-torch-1.4.0_0.5.0-hostpath-pv-cpu
TORCH_LOAD_BALANCER_SERVICE_GPU=loadbalancer-torch-1.4.0_0.5.0-hostpath-pv-gpu
TORCH_PV=hostpath-pv
TORCH_PVC=hostpath-pvc
TORCH_JUPYTERLAB_SERVICE_CPU=jupyterlab-torch-1.4.0_0.5.0-hostpath-pv-cpu
TORCH_JUPYTERLAB_SERVICE_GPU=jupyterlab-torch-1.4.0_0.5.0-hostpath-pv-gpu
TORCH_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/torch/
```

Next lines are not necessary but help to modify services:

```
wget $TORCH_URL/hostpath_pv/$TORCH_LOAD_BALANCER_SERVICE_CPU.yaml
wget $TORCH_URL/hostpath_pv/$TORCH_LOAD_BALANCER_SERVICE_GPU.yaml
wget $TORCH_URL/hostpath_pv/$TORCH_PV.yaml
wget $TORCH_URL/hostpath_pv/$TORCH_PVC.yaml
wget $TORCH_URL/hostpath_pv/$TORCH_JUPYTERLAB_SERVICE_CPU.yaml
wget $TORCH_URL/hostpath_pv/$TORCH_JUPYTERLAB_SERVICE_GPU.yaml
```

Create storage:

```
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_PV.yaml
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_PVC.yaml
```

**Next for GPU deployment (for testing notebooks):**

Create service:

```
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_LOAD_BALANCER_SERVICE_GPU.yaml
```

Create deployment:

```
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_JUPYTERLAB_SERVICE_GPU.yaml
```

To check set:

```
TORCH_LOAD_BALANCER_SERVICE_GPU=$(echo $TORCH_LOAD_BALANCER_SERVICE_GPU|sed -n 's/\./-/g;s/_/-/g;p')
TORCH_JUPYTERLAB_SERVICE_GPU=$(echo $TORCH_JUPYTERLAB_SERVICE_GPU|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $TORCH_LOAD_BALANCER_SERVICE_GPU
kubectl describe pv -n kubeflow $TORCH_PV
kubectl describe pvc -n kubeflow $TORCH_PVC
kubectl describe deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_GPU
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_GPU --replicas=<0 or 1>
```


Delete:

```
kubectl delete service -n kubeflow $TORCH_LOAD_BALANCER_SERVICE_GPU
kubectl delete pvc -n kubeflow $TORCH_PVC
kubectl delete pv -n kubeflow $TORCH_PV
kubectl delete deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_GPU 
```

**Next for CPU deployment and launch via kale:**


Create service:

```
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_LOAD_BALANCER_SERVICE_CPU.yaml
```

Create deployment:

```
kubectl create -f $TORCH_URL/hostpath_pv/$TORCH_JUPYTERLAB_SERVICE_CPU.yaml
```

To check set:

```
TORCH_LOAD_BALANCER_SERVICE_CPU=$(echo $TORCH_LOAD_BALANCER_SERVICE_CPU|sed -n 's/\./-/g;s/_/-/g;p')
TORCH_JUPYTERLAB_SERVICE_CPU=$(echo $TORCH_JUPYTERLAB_SERVICE_CPU|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $TORCH_LOAD_BALANCER_SERVICE_CPU
kubectl describe pv -n kubeflow $TORCH_PV
kubectl describe pvc -n kubeflow $TORCH_PVC
kubectl describe deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_CPU
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_CPU --replicas=<0 or 1>
```


Delete:

```
kubectl delete service -n kubeflow $TORCH_LOAD_BALANCER_SERVICE_CPU
kubectl delete pvc -n kubeflow $TORCH_PVC
kubectl delete pv -n kubeflow $TORCH_PV
kubectl delete deployment -n kubeflow $TORCH_JUPYTERLAB_SERVICE_CPU 
```
