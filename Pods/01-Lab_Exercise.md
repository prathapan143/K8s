### **Lab Exercise 1: Create a Basic Pod**

**Objective**: Create a pod running the `nginx` image.

**Steps**:
1. Write a YAML file `nginx-pod.yaml`:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx-pod
    spec:
      containers:
      - name: nginx
        image: nginx:latest
    ```

2. Create the pod:
   ```bash
   kubectl create -f nginx-pod.yaml
   ```

3. Check the pod is running:
   ```bash
   kubectl get pods
   ```

**Solution**: You should see the `nginx-pod` in the list of running pods.

---

### **Lab Exercise 2: Expose Pod with a Port**

**Objective**: Create a pod running `nginx` image and expose it on port `8080`.

**Steps**:
1. Write a YAML file `nginx-pod-port.yaml`:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx-pod-port
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
          hostPort: 8080
    ```

2. Create the pod:
   ```bash
   kubectl create -f nginx-pod-port.yaml
   ```

3. Check the pod is running:
   ```bash
   kubectl get pods
   ```

4. Access the Nginx welcome page by browsing to `http://<Node-IP>:8080`.

**Solution**: The `nginx` welcome page should be displayed.

---

### **Lab Exercise 3: Add Environment Variables to a Pod**

**Objective**: Create a pod running an `nginx` image with environment variables.

**Steps**:
1. Write a YAML file `nginx-pod-env.yaml`:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx-pod-env
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        env:
        - name: ENV_VAR_NAME
          value: "Hello, Kubernetes!"
    ```

2. Create the pod:
   ```bash
   kubectl create -f nginx-pod-env.yaml
   ```

3. Check the environment variables inside the pod:
   ```bash
   kubectl exec nginx-pod-env env
   ```

**Solution**: You should see `ENV_VAR_NAME=Hello, Kubernetes!` in the list of environment variables.

---

### **Lab Exercise 4: Create a Multi-container Pod**

**Objective**: Create a pod that has both `nginx` and `busybox` containers. The `busybox` container should run in a loop pinging the `nginx` container.

**Steps**:
1. Write a YAML file `multi-container-pod.yaml`:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: multi-container-pod
    spec:
      containers:
      - name: nginx
        image: nginx:latest
      - name: busybox
        image: busybox
        command: ["/bin/sh"]
        args: ["-c", "while true; do wget -q -O- http://localhost; sleep 2; done"]
    ```

2. Create the pod:
   ```bash
   kubectl create -f multi-container-pod.yaml
   ```

3. Check the logs of the `busybox` container:
   ```bash
   kubectl logs multi-container-pod busybox
   ```

**Solution**: You should see the `nginx` welcome page HTML in the logs.

---

Each of these exercises will help you grasp the fundamentals of creating and manipulating pods in a Kubernetes cluster. Make sure you clean up your resources after each exercise with `kubectl delete pod <POD_NAME>` to avoid any conflicts.
