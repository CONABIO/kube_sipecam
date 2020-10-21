# For Kubeflow namespace:

Set:

```
ECOINF_LOAD_BALANCER_SERVICE=loadbalancer-ecoinf-0.5.1-hostpath-pv
ECOINF_PV=hostpath-pv
ECOINF_PVC=hostpath-pvc
ECOINF_JUPYTERLAB_SERVICE=jupyterlab-ecoinf-0.5.1-hostpath-pv
ECOINF_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/ecoinf/
```

Next lines are not necessary but help to modify services:

```
wget $ECOINF_URL/hostpath_pv/$ECOINF_LOAD_BALANCER_SERVICE.yaml
wget $ECOINF_URL/hostpath_pv/$ECOINF_PV.yaml
wget $ECOINF_URL/hostpath_pv/$ECOINF_PVC.yaml
wget $ECOINF_URL/hostpath_pv/$ECOINF_JUPYTERLAB_SERVICE.yaml
```

Create storage:

```
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_PV.yaml
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_PVC.yaml
```

Create service:

```
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $ECOINF_URL/hostpath_pv/$ECOINF_JUPYTERLAB_SERVICE.yaml
```

To check set:

```
ECOINF_LOAD_BALANCER_SERVICE=$(echo $ECOINF_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
ECOINF_JUPYTERLAB_SERVICE=$(echo $ECOINF_JUPYTERLAB_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $ECOINF_LOAD_BALANCER_SERVICE
kubectl describe pv -n kubeflow $ECOINF_PV
kubectl describe pvc -n kubeflow $ECOINF_PVC
kubectl describe deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $ECOINF_LOAD_BALANCER_SERVICE
kubectl delete pvc -n kubeflow $ECOINF_PVC
kubectl delete pv -n kubeflow $ECOINF_PV
kubectl delete deployment -n kubeflow $ECOINF_JUPYTERLAB_SERVICE
```
