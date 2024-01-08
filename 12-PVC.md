Persistent Volume Claims (PVCs) are used to request storage resources from a Persistent Volume (PV). Here's a step-by-step guide on how to create a PVC and assign it to pods:
---

### Step 1: Create a Persistent Volume (PV)
Before creating a PVC, you need to have a Persistent Volume available. Here's an example of a simple PV configuration:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

Save this configuration to a file, for example, `pv.yaml`, and apply it using:

```bash
kubectl apply -f pv.yaml
```

### Step 2: Create a Persistent Volume Claim (PVC)
Now, you can create a PVC that will request storage from the PV:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Save this configuration to a file, for example, `pvc.yaml`, and apply it using:

```bash
kubectl apply -f pvc.yaml
```

### Step 3: Create a Pod using the PVC
Now, you can create a pod that uses the PVC by referencing the PVC in the `volumes` section and mounting the volume in the `containers` section:

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
    - name: storage
      mountPath: "/usr/share/nginx/html"
  volumes:
  - name: storage
    persistentVolumeClaim:
      claimName: my-pvc
```

Save this configuration to a file, for example, `pod.yaml`, and apply it using:

```bash
kubectl apply -f pod.yaml
```

This example assumes you have a simple NGINX container using the storage provided by the PVC. Adjust the configurations based on your specific use case and requirements.
