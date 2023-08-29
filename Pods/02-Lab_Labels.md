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




Sure, here are 4 lab exercises with solutions for Kubernetes Labels:

## Exercise 1: Creating and Updating Labels

### Objective:
In this exercise, you will learn how to create and update labels for a Kubernetes resource.

### Steps:
1. Create a new deployment called `my-deployment` with the image `nginx:latest`.
2. Add the label `app: web` to the deployment.
3. Update the label to `app: frontend`.

### Solution:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

To update the label, simply edit the deployment and change `app: web` to `app: frontend`.

## Exercise 2: Selecting Resources by Label

### Objective:
In this exercise, you will learn how to select Kubernetes resources by label.

### Steps:
1. Create a new deployment called `my-deployment` with the image `nginx:latest`.
2. Add the label `app: web` to the deployment.
3. Create a new service called `my-service` that selects the `my-deployment` by label.

### Solution:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest

---

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: web
  ports:
  - name: http
    port: 80
    targetPort: 80
```

## Exercise 3: Labeling Nodes

### Objective:
In this exercise, you will learn how to label Kubernetes nodes.

### Steps:
1. Get the name of a node in your cluster using `kubectl get nodes`.
2. Add the label `environment: production` to the node.

### Solution:
```bash
$ kubectl get nodes
NAME          STATUS   ROLES                  AGE   VERSION
my-node       Ready    control-plane,master   2d    v1.22.1

$ kubectl label nodes my-node environment=production
```

## Exercise 4: Using Labels in Deployments

### Objective:
In this exercise, you will learn how to use labels in Kubernetes deployments.

### Steps:
1. Create a new deployment called `my-deployment` with the image `nginx:latest`.
2. Add the label `app: web` to the deployment.
3. Update the deployment to use a rolling update strategy with a max surge of 1 and a max unavailable of 0.
4. Scale up the deployment to 3 replicas.

### Solution:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  selector:
    matchLabels:
      app: web
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: nginx
        image: nginx:latest
```

Note that the update strategy and scaling can be done separately by editing the deployment.
