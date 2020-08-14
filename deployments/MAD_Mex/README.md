# For Kubeflow namespace:

Set:

```
KALE_LOAD_BALANCER_SERVICE=kale-service-kubeflow_0.5.0_0.1.0
KALE_JUPYTERLAB_SERVICE=kale-jupyterlab-kubeflow_0.5.0_0.1.0
```


Create service:

```
kubectl create -f $KALE_LOAD_BALANCER_SERVICE.yaml
```

Create deployment:

```
kubectl create -f $KALE_JUPYTERLAB_SERVICE.yaml
```

Check:

```
kubectl describe service $KALE_LOAD_BALANCER_SERVICE.yaml
kubectl describe service $KALE_JUPYTERLAB_SERVICE.yaml
```

```
kubectl describe pods
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment $KALE_LOAD_BALANCER_SERVICE --replicas=<0 or 1>
kubectl scale deployment $KALE_JUPYTERLAB_SERVICE --replicas=<0 or 1>
```

Delete:

```
kubectl delete service $KALE_LOAD_BALANCER_SERVICE
kubectl delete service $KALE_JUPYTERLAB_SERVICE
```
