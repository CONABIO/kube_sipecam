# For Kubeflow namespace:

Create service:

```
kubectl create -f kale-service-kubeflow_0.4.0_1.14.0_tf.yaml
kubectl create -f kale-service-kubeflow_0.4.0_1.14.0_tf_cpu.yaml
```

Create deployment:

```
kubectl create -f kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf.yaml
kubectl create -f kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf_cpu.yaml
```

Check:

```
kubectl describe service kale-service-kubeflow_0.4.0_1.14.0_tf
kubectl describe service kale-service-kubeflow_0.4.0_1.14.0_tf_cpu
```

```
kubectl describe pods
```

Scale: (if created automatically scale is 1)

```
kubectl scale deployment kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf --replicas=<0 or 1>
kubectl scale deployment kale-service-kubeflow_0.4.0_1.14.0_tf_cpu --replicas=<0 or 1>
```

Delete:

```
kubectl delete service kale-service-kubeflow_0.4.0_1.14.0_tf
kubectl delete service kale-service-kubeflow_0.4.0_1.14.0_tf_cpu
```

```
kubectl delete deployment kale-jupyterlab-kubeflow_0.4.0_1.14.0_tf
kubectl delete deployment kale-service-kubeflow_0.4.0_1.14.0_tf_cpu
```
