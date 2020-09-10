# For Kubeflow namespace:

Set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=loadbalancer-mad-mex-0.5.0_0.1.0
MAD_MEX_JUPYTERLAB_SERVICE=jupyterlab-mad-mex-0.5.0_0.1.0
MAD_MEX_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/MAD_Mex/
```

Next lines are not necessary but help to modify services:

```
wget $MAD_MEX_URL$MAD_MEX_LOAD_BALANCER_SERVICE.yaml
wget $MAD_MEX_URL$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

Create service:

```
kubectl create -f $MAD_MEX_URL$MAD_MEX_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $MAD_MEX_URL$MAD_MEX_JUPYTERLAB_SERVICE.yaml
```

To check set:

```
MAD_MEX_LOAD_BALANCER_SERVICE=$(echo $MAD_MEX_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
MAD_MEX_JUPYTERLAB_SERVICE=$(echo $MAD_MEX_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $MAD_MEX_LOAD_BALANCER_SERVICE
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
kubectl delete deployment -n kubeflow $MAD_MEX_JUPYTERLAB_SERVICE
```
