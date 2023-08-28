Sure, here are lab exercises with solutions for Kubernetes ConfigMap:

1. Create a ConfigMap with a single key-value pair.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  greeting: Hello, world!
```

2. Create a Pod that uses the ConfigMap to set an environment variable.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    env:
    - name: GREETING
      valueFrom:
        configMapKeyRef:
          name: my-configmap
          key: greeting
```

3. Create a Pod that uses the ConfigMap to mount a file in a read-only volume.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    volumeMounts:
    - name: my-configmap
      mountPath: /etc/config
      readOnly: true
  volumes:
  - name: my-configmap
    configMap:
      name: my-configmap
```

4. Create a Pod that uses the ConfigMap to read data using the Kubernetes API.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx
    command: ["/bin/sh", "-c", "echo $(kubectl get configmap my-configmap -o jsonpath='{.data.greeting}')"]
```

5. Update the ConfigMap and verify that the Pod is updated.

```
kubectl edit configmap my-configmap
```

Change the value of the `greeting` key to `Goodbye, world!`.

```
kubectl get pod my-pod
```

The output should now show the new greeting value.

These are just a few of the many things you can do with ConfigMaps in Kubernetes. For more information, please see the Kubernetes documentation on ConfigMaps: https://kubernetes.io/docs/concepts/configuration/configmap/.

I hope this helps! Let me know if you have any other questions.
