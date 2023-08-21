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



# Lab Exercise: Kubernetes Deployment for Image Update

**Objective**: This lab exercise will guide you through updating the container image of a Kubernetes deployment.

**Prerequisites**:
- A running Kubernetes cluster (Minikube or any cloud provider's Kubernetes service).
- `kubectl` CLI tool installed and configured to interact with your cluster.

## Steps:

### 1. Create a Sample Deployment

If you don't already have a deployment to work with, let's create a sample deployment:

```bash
kubectl create deployment nginx-deployment --image=nginx:1.17.10
```

This creates a deployment named `nginx-deployment` using the `nginx:1.17.10` image.

### 2. Verify the Deployment

To ensure the deployment was created successfully:

```bash
kubectl get deployments
```

### 3. Update the Image for the Deployment

To update the image for our deployment:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.6
```

This updates the image for the `nginx-deployment` to `nginx:1.19.6`.

### 4. Check the Status of the Deployment Update

To observe the rolling update:

```bash
kubectl rollout status deployment/nginx-deployment
```

### 5. Verify the Updated Image

To check the image of the running container(s):

```bash
kubectl get deployment nginx-deployment -o=jsonpath='{$.spec.template.spec.containers[:1].image}'
```

This should show `nginx:1.19.6`.

### 6. Rollback (Optional)

If there's any issue with the new version, Kubernetes allows you to roll back to the previous version:

```bash
kubectl rollout undo deployment/nginx-deployment
```

This rolls back the deployment to the previous version.

### 7. Cleanup

Once you've completed the exercise, clean up the resources:

```bash
kubectl delete deployment nginx-deployment
```

## Conclusion

At this point, you should have successfully updated the container image of a Kubernetes deployment, verified the update, and optionally rolled back to a previous version. Updating container images is a fundamental operation in Kubernetes, and mastering it will help you manage and maintain applications effectively.



# Lab Exercise: Kubernetes Deployment with `--revision`

**Objective**: In this lab exercise, you will dive deep into Kubernetes' deployment mechanism and learn how to inspect specific revisions of a deployment and roll back to them if necessary.

**Prerequisites**:
- A running Kubernetes cluster (Minikube or any cloud provider's Kubernetes service).
- `kubectl` CLI tool installed and configured to interact with your cluster.

## Steps:

### 1. Create a Sample Deployment

If you don't already have a deployment to work with, let's create a sample deployment:

```bash
kubectl create deployment nginx-deployment --image=nginx:1.17.10
```

This creates a deployment named `nginx-deployment` using the `nginx:1.17.10` image.

### 2. Verify the Deployment

To ensure the deployment was created successfully:

```bash
kubectl get deployments
```

### 3. Update the Deployment's Image

Let's make a few updates to the deployment's image to generate multiple revisions:

First update:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.1
```

Second update:

```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.19.2
```

### 4. Inspect Deployment Revisions

To list the rollout history of our deployment:

```bash
kubectl rollout history deployment/nginx-deployment
```

You should see two revisions listed with their respective change-cause.

To inspect details of a specific revision, use:

```bash
kubectl rollout history deployment/nginx-deployment --revision=1
```

Replace `1` with the revision number you're interested in.

### 5. Roll Back to a Specific Revision

Let's say you want to roll back to the first revision:

```bash
kubectl rollout undo deployment/nginx-deployment --to-revision=1
```

### 6. Verify the Rollback

To check the image of the running container(s) post-rollback:

```bash
kubectl get deployment nginx-deployment -o=jsonpath='{$.spec.template.spec.containers[:1].image}'
```

This should show `nginx:1.17.10`.

### 7. Cleanup

Once you've completed the exercise, clean up the resources:

```bash
kubectl delete deployment nginx-deployment
```

## Conclusion

You have successfully explored the revision-specific aspects of a Kubernetes deployment. Understanding how to inspect and roll back to specific revisions is crucial for managing applications in production and ensuring reliability.
