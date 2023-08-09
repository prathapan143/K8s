You can manually schedule a pod on a specific node by specifying the `nodeName` field in the Pod specification. When a `nodeName` is given, the Kubernetes scheduler is bypassed and the pod is directly assigned to run on the node with the given name.

Here is an example Pod spec with `nodeName`:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: manually-scheduled-pod
spec:
  nodeName: specific-node-name
  containers:
  - name: my-container
    image: my-image
```

In this YAML:

- `nodeName` is set to `specific-node-name`, which is the name of the node where you want to schedule the Pod.

- `name` in `containers` is the name of your container.
  
- `image` in `containers` is the Docker image of your container.

You would save this YAML in a file (say `pod.yaml`) and create the Pod using:

```bash
kubectl apply -f pod.yaml
```

Please note that specifying a `nodeName` in the Pod spec is a form of direct pod scheduling. It bypasses the Kubernetes scheduler which means it does not take into account the various sophisticated scheduling features like resource-based scheduling, affinity and anti-affinity, taints and tolerations etc.

It's typically best to let Kubernetes automatically handle scheduling so it can balance resources across all nodes in the cluster. Manual scheduling is generally used for specific scenarios where you need fine-grained control.
