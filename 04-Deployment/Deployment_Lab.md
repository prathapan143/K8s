---

### Lab Exercise 1: Create a Basic Deployment

**Objective**: Deploy an nginx server using a Kubernetes Deployment.

1. **Task**: Define a Deployment manifest for nginx with the name "nginx-deploy" and 2 replicas.

**Solution**:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
spec:
  replicas: 2
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

### Lab Exercise 2: Update Deployment Image

**Objective**: Update the nginx server version.

1. **Task**: Update the nginx image version in the "nginx-deploy" Deployment to `nginx:1.17.9`.

**Solution**:
Update the Deployment manifest with the new image and then apply it using `kubectl apply`.

---

### Lab Exercise 3: Rollback Deployment

**Objective**: Rollback the Deployment to a previous version.

1. **Task**: Roll back the "nginx-deploy" Deployment to its previous version after updating the image in Exercise 2.

**Solution**:
```bash
kubectl rollout undo deployment/nginx-deploy
```

---

### Lab Exercise 4: Scale Deployment

**Objective**: Manually scale the Deployment.

1. **Task**: Scale the "nginx-deploy" Deployment to 5 replicas without modifying the manifest file.

**Solution**:
```bash
kubectl scale deployment nginx-deploy --replicas=5
```

---

### Lab Exercise 5: Pausing and Resuming Deployment

**Objective**: Learn to pause a deployment, update it, and then resume it.

1. **Task**:
   - Pause the "nginx-deploy" Deployment.
   - Update the nginx image to `nginx:1.18.0` without causing an immediate rollout.
   - Resume the deployment and watch the rollout of the new version.

**Solution**:
1. Pause the Deployment:
```bash
kubectl rollout pause deployment/nginx-deploy
```

2. Update the Deployment (for example, by modifying the manifest and applying it):
```yaml
# ... (rest of the manifest remains unchanged)
        spec:
          containers:
          - name: nginx
            image: nginx:1.18.0
```

3. Resume the Deployment:
```bash
kubectl rollout resume deployment/nginx-deploy
```

---

To validate the results after each exercise, learners can use `kubectl get deploy`, `kubectl describe deploy <deployment-name>`, and other relevant `kubectl` commands.
