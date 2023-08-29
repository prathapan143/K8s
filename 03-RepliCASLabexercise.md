lab exercises on Kubernetes ReplicaSet with their solutions:

---

### Lab Exercise 1: Create a Simple ReplicaSet

**Objective**: Create a ReplicaSet to manage 3 replicas of an nginx pod.

1. **Task**: Define a ReplicaSet manifest for nginx with the name "nginx-rs" and 3 replicas.

**Solution**:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
```

---

### Lab Exercise 2: Update the ReplicaSet

**Objective**: Update the existing ReplicaSet to use a newer version of the nginx image.

1. **Task**: Update the nginx image version in the ReplicaSet "nginx-rs" to `nginx:1.17.9`.

**Solution**:
1. Update the ReplicaSet manifest:
```yaml
# ... (rest of the manifest remains unchanged)
        spec:
          containers:
          - name: nginx
            image: nginx:1.17.9
```

2. Apply the changes:
```bash
kubectl apply -f [path_to_updated_manifest_file].yaml
```

---

### Lab Exercise 3: Scale the ReplicaSet

**Objective**: Manually scale the "nginx-rs" ReplicaSet to 5 replicas.

1. **Task**: Scale the number of replicas in the "nginx-rs" ReplicaSet to 5 without modifying the manifest file.

**Solution**:
Use the `kubectl scale` command:
```bash
kubectl scale replicaset nginx-rs --replicas=5
```

---

After completing each lab exercise, users can verify their work using `kubectl get rs` and `kubectl describe rs <replicaset-name>`.
