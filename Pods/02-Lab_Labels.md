 Kubernetes labels are key-value pairs associated with resources, such as pods, to help organize and select subsets of objects. Here are four lab exercises with solutions related to Kubernetes labels:

### Lab Exercise 1: Assign Labels to Pods

#### Objective:
Assign a label `tier: frontend` to a running pod.

#### Steps:
1. Create a basic pod:
   ```bash
   kubectl run nginx --image=nginx
   ```

2. List the running pods:
   ```bash
   kubectl get pods
   ```

3. Assign a label to the `nginx` pod:
   ```bash
   kubectl label pods nginx tier=frontend
   ```

#### Solution:
Verify the label is attached to the pod:
```bash
kubectl get pods --show-labels
```

---

### Lab Exercise 2: Filter Pods using Labels

#### Objective:
Use label selectors to filter and list pods labeled as `tier: frontend`.

#### Steps:
1. Assuming you have multiple pods and one of them is labeled with `tier: frontend`, filter the pods with this label:
   ```bash
   kubectl get pods -l tier=frontend
   ```

#### Solution:
You should see a list of pods with the label `tier: frontend`.

---

### Lab Exercise 3: Remove Labels from Resources

#### Objective:
Remove the label `tier: frontend` from a pod.

#### Steps:
1. To remove the label from the pod:
   ```bash
   kubectl label pods nginx tier-
   ```

#### Solution:
To confirm that the label has been removed:
```bash
kubectl get pods --show-labels
```

---

### Lab Exercise 4: Assign Multiple Labels and Select Using Multiple Criteria

#### Objective:
Assign multiple labels to a pod and then use label selectors to filter using multiple criteria.

#### Steps:
1. Assign multiple labels:
   ```bash
   kubectl label pods nginx tier=backend environment=production
   ```

2. List pods that are labeled as `backend` and belong to the `production` environment:
   ```bash
   kubectl get pods -l tier=backend,environment=production
   ```

#### Solution:
You should see the `nginx` pod in the output since it matches both criteria.

These exercises should help learners understand the basic functionality of Kubernetes labels and how to use them effectively.
