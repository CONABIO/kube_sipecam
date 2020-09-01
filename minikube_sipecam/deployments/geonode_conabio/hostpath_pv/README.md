# For Kubeflow namespace:

Set:

```
GEONODE_CONABIO_LOAD_BALANCER_SERVICE=loadbalancer-geonode-conabio-0.1_0.5.0-hostpath-pv
GEONODE_CONABIO_PV=hostpath-pv
GEONODE_CONABIO_PVC=hostpath-pvc
GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV=jupyterlab-geonode-conabio-0.1_0.5.0-hostpath-pv
GEONODE_CONABIO_URL=https://raw.githubusercontent.com/CONABIO/kube_sipecam/master/minikube_sipecam/deployments/geonode_conabio/
```

Next lines are not necessary but help to modify services:

```
wget $GEONODE_CONABIO_URL$GEONODE_CONABIO_LOAD_BALANCER_SERVICE.yaml
wget $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_PV.yaml
wget $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_PVC.yaml
wget $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

Create storage:

```
kubectl create -f $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_PV.yaml
kubectl create -f $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_PVC.yaml
```

Create service:

```
kubectl create -f $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $GEONODE_CONABIO_URL/hostpath_pv/$GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV.yaml
```

To check set:

```
GEONODE_CONABIO_LOAD_BALANCER_SERVICE=$(echo $GEONODE_CONABIO_LOAD_BALANCER_SERVICE|sed -n 's/\./-/g;s/_/-/g;p')
GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV=$(echo $GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV|sed -n 's/\./-/g;s/_/-/g;p')
```

Describe:

```
kubectl describe service -n kubeflow $GEONODE_CONABIO_LOAD_BALANCER_SERVICE
kubectl describe pv -n kubeflow $GEONODE_CONABIO_PV
kubectl describe pvc -n kubeflow $GEONODE_CONABIO_PVC
kubectl describe deployment -n kubeflow $GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV
```

Describe all pods:

```
kubectl describe pods -n kubeflow
```

Scale: (if created automatically scale is 1)

```
kubectl scale service -n kubeflow $GEONODE_CONABIO_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment -n kubeflow $GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV --replicas=<0 or 1>
```

Delete:

```
kubectl delete service -n kubeflow $GEONODE_CONABIO_LOAD_BALANCER_SERVICE
kubectl delete pvc -n kubeflow $GEONODE_CONABIO_PVC
kubectl delete pv -n kubeflow $GEONODE_CONABIO_PV
kubectl delete deployment -n kubeflow $GEONODE_CONABIO_JUPYTERLAB_SERVICE_HOSTPATH_PV 
```
