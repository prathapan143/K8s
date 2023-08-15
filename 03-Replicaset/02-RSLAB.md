## Lab Exercise 1: Creating a Replicaset

**Objective:** Create a Replicaset with three replicas and verify that the pods are running.

**Solution:**

1. Create a YAML file for the Replicaset:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx
```

2. Apply the YAML file to create the Replicaset:

```
kubectl apply -f replicaset.yaml
```

3. Verify that the Replicaset and its pods are running:

```
kubectl get rs
kubectl get pods -l app=my-app
```

## Lab Exercise 2: Scaling a Replicaset

**Objective:** Scale up a Replicaset to five replicas and verify that the pods are running.

**Solution:**

1. Scale up the Replicaset to five replicas:

```
kubectl scale rs my-replicaset --replicas=5
```

2. Verify that the Replicaset and its pods are running:

```
kubectl get rs
kubectl get pods -l app=my-app
```

## Lab Exercise 3: Updating a Replicaset

**Objective:** Update the image of a Replicaset and verify that the pods are updated.

**Solution:**

1. Update the image of the Replicaset to `nginx:1.19.10`:

```
kubectl set image rs my-replicaset my-container=nginx:1.19.10
```

2. Verify that the pods are updated:

```
kubectl get pods -l app=my-app -o jsonpath='{range .items[*]}{.spec.containers[0].image}{"\n"}{end}'
``` 

The output should show `nginx:1.19.10` for all pods in the Replicaset.

I hope these lab exercises help you learn more about Kubernetes Replicasets! Let me know if you have any questions.
