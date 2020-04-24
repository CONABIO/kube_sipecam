# For Kubeflow namespace:

Create service:

```
kubectl create -f kale-service-kubeflow_0.4.0_1.14.0_tf.yaml
kubectl create -f tfx-service-kubeflow_1.14.0_tfx.yaml
```

Create deployment:

```
kubectl create -f kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf.yaml
kubectl create -f tfx-jupyterlab-kubeflow_1.14.0_tfx.yaml
```

Check:

```
kubectl describe service kale-service-kubeflow_0.4.0_1.14.0_tf.yaml
kubectl describe service tfx-service-kubeflow_1.14.0_tfx.yaml
```

```
kubectl describe pods
```

Scale:

```
kubectl scale deployment kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf --replicas=<0 or 1>
kubectl scale deployment tfx-service-kubeflow_1.14.0_tfx --replicas=<0 or 1>
```

Delete:

```
kubectl delete service kale-service-kubeflow_0.4.0_1.14.0_tf
kubectl delete service tfx-service-kubeflow_1.14.0_tfx
```

```
kubectl delete deployment kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf
kubectl delete deployment tfx-service-kubeflow_1.14.0_tfx
```
