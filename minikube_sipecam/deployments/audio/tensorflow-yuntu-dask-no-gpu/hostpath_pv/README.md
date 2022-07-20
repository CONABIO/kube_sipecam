# For Kubeflow namespace:

Set:

```
AUDIO_KUBEFLOW_NAMESPACE=kubeflow-namespace
AUDIO_LOAD_BALANCER_SERVICE_NO_GPU=loadbalancer-audio-tf-yuntu-dask-no-gpu-0.6.1-hostpath-pv
AUDIO_PV=hostpath-pv
AUDIO_PVC=hostpath-pvc
AUDIO_PV_EFS=hostpath-pv-efs
AUDIO_PVC_EFS=hostpath-pvc-efs
AUDIO_JUPYTERLAB_SERVICE_NO_GPU=jupyterlab-audio-tf-yuntu-dask-no-gpu-0.6.1-hostpath-pv
AUDIO_URL_NO_GPU=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/audio/tensorflow-yuntu-dask-no-gpu
```

Next lines are not necessary but help to modify services:

```
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_LOAD_BALANCER_SERVICE_NO_GPU.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PV.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PVC.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PV_EFS.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PVC_EFS.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_KUBEFLOW_NAMESPACE.yaml
wget $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_JUPYTERLAB_SERVICE_NO_GPU.yaml
```

Create kubeflow namespace:

```
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_KUBEFLOW_NAMESPACE.yaml
```

Create storage:

```
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PV.yaml
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PVC.yaml
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PV_EFS.yaml
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_PVC_EFS.yaml
```


Create service:

```
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_LOAD_BALANCER_SERVICE_NO_GPU.yaml
```

Create deployment:

```
kubectl create -f $AUDIO_URL_NO_GPU/hostpath_pv/$AUDIO_JUPYTERLAB_SERVICE_NO_GPU.yaml
```

To check set:

```
AUDIO_LOAD_BALANCER_SERVICE_GPU=$(echo $AUDIO_LOAD_BALANCER_SERVICE_NO_GPU|sed -n 's/\./-/g;s/_/-/g;p')
AUDIO_JUPYTERLAB_SERVICE_GPU=$(echo $AUDIO_JUPYTERLAB_SERVICE_NO_GPU|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $AUDIO_LOAD_BALANCER_SERVICE_NO_GPU
kubectl describe pv -n kubeflow $AUDIO_PV
kubectl describe pvc -n kubeflow $AUDIO_PVC
kubectl describe pv -n kubeflow $AUDIO_PV_EFS
kubectl describe pvc -n kubeflow $AUDIO_PVC_ES
kubectl describe deployment -n kubeflow $AUDIO_JUPYTERLAB_SERVICE_NO_GPU
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $AUDIO_JUPYTERLAB_SERVICE_NO_GPU --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $AUDIO_LOAD_BALANCER_SERVICE_NO_GPU
kubectl delete pvc -n kubeflow $AUDIO_PVC
kubectl delete pv -n kubeflow $AUDIO_PV
kubectl describe pv -n kubeflow $AUDIO_PV_EFS
kubectl describe pvc -n kubeflow $AUDIO_PVC_ES
kubectl delete deployment -n kubeflow $AUDIO_JUPYTERLAB_SERVICE_NO_GPU
```
