Sure! Here are five lab exercises related to Kubernetes Secrets, along with their solutions:

### Exercise 1: Create a Secret Manually

**Task**: Create a Secret named `api-credentials` with two key-value pairs: `api-key` with value `12345` and `api-secret` with value `abcdef`.

**Solution**:
```bash
kubectl create secret generic api-credentials --from-literal=api-key=12345 --from-literal=api-secret=abcdef
```

### Exercise 2: Use Secret in a Pod as Environment Variables

**Task**: Create a Pod named `secret-env-pod` which uses the Secret `api-credentials` to set two environment variables: `API_KEY` and `API_SECRET`.

**Solution**:

Pod YAML (`secret-env-pod.yaml`):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-pod
spec:
  containers:
  - name: my-container
    image: busybox
    env:
    - name: API_KEY
      valueFrom:
        secretKeyRef:
          name: api-credentials
          key: api-key
    - name: API_SECRET
      valueFrom:
        secretKeyRef:
          name: api-credentials
          key: api-secret
    command: [ "sleep", "3600" ]
```

Command:
```bash
kubectl apply -f secret-env-pod.yaml
```

### Exercise 3: Mount Secret as a Volume in a Pod

**Task**: Create a Pod named `secret-volume-pod` that mounts the Secret `api-credentials` in the directory `/etc/credentials`.

**Solution**:

Pod YAML (`secret-volume-pod.yaml`):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-pod
spec:
  containers:
  - name: my-container
    image: busybox
    volumeMounts:
    - name: creds-volume
      mountPath: /etc/credentials
    command: [ "sleep", "3600" ]
  volumes:
  - name: creds-volume
    secret:
      secretName: api-credentials
```

Command:
```bash
kubectl apply -f secret-volume-pod.yaml
```

### Exercise 4: Create a Secret from a File

**Task**: Given a file `db.conf` with content `user=root\npassword=secret`, create a Secret named `db-config` using this file.

**Solution**:

Command:
```bash
kubectl create secret generic db-config --from-file=db.conf
```

### Exercise 5: Update a Secret

**Task**: Update the `api-key` in the `api-credentials` Secret to a new value `67890`.

**Solution**:

1. First, fetch the current Secret, edit, and reapply:

```bash
kubectl get secret api-credentials -o yaml > api-credentials.yaml
```

2. Edit the `api-credentials.yaml` file and replace the base64 encoded value of `api-key`. Use the following command to get the base64 value:

```bash
echo -n "67890" | base64
```

3. Replace the old base64 value with the new one in the `api-credentials.yaml` file.

4. Apply the updated Secret:

```bash
kubectl apply -f api-credentials.yaml
```

Note: For all the above exercises, after creating or updating resources, you can validate your work using commands like `kubectl describe` or `kubectl get` to inspect the Secret or Pod configurations.
