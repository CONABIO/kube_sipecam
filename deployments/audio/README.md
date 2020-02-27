Create service:

```
kubectl create -f kale-service.yaml
```

Create deployment:

```
kubectl create -f kale-jupyterlab.yaml
```

Check:

```
kubectl describe service kale-service-jupyterlab
```

```
kubectl describe pods
```

Scale:

```
kubectl scale deployment kale-jupyterlab --replicas=<0 or 1>
```

Delete:

```
kubectl delete service kale-service-jupyterlab
```

```
kubectl delete deployment kale-jupyterlab
```
