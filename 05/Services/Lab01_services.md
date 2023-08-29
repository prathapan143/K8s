
### Exercise 1: Deploy a simple web application using NodePort

**Objective:** Deploy a web application and expose it using NodePort.

**Steps:**
1. Create a Deployment for an Nginx web server.
2. Expose the Deployment using a Service of type NodePort.

**Solution:**
```bash
# 1. Create the Nginx Deployment
kubectl create deployment nginx --image=nginx

# 2. Expose the Deployment using a NodePort Service
kubectl expose deployment nginx --type=NodePort --port=80
```

### Exercise 2: Access the application through NodePort

**Objective:** Access the web application exposed in Exercise 1 from outside the cluster.

**Steps:**
1. Identify the node's IP address and the service's NodePort.
2. Access the application using a web browser or `curl`.

**Solution:**
```bash
# 1. Get the NodePort assigned to the service
NODE_PORT=$(kubectl get svc/nginx -o=jsonpath='{.spec.ports[0].nodePort}')

# 2. Get the node's IP address (this might depend on the environment)
NODE_IP=$(kubectl get nodes -o=jsonpath='{.items[0].status.addresses[?(@.type=="ExternalIP")].address}')

# 3. Access the web application (using curl for this example)
curl $NODE_IP:$NODE_PORT
```

### Exercise 3: Set a specific NodePort for a service

**Objective:** Deploy a web application and set a specific NodePort for the service.

**Steps:**
1. Deploy another version of Nginx.
2. Create a Service definition YAML with a specified NodePort.

**Solution:**
```yaml
# nginx-nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-fixed-port
spec:
  type: NodePort
  ports:
    - port: 80
      nodePort: 30007
  selector:
    app: nginx
```

```bash
# Apply the service definition
kubectl apply -f nginx-nodeport.yaml
```

### Exercise 4: Change the NodePort of an existing service

**Objective:** Update an existing NodePort Service to use a different port.

**Steps:**
1. Edit the existing NodePort Service.
2. Change the NodePort to a different value.

**Solution:**
```bash
# Edit the service directly using kubectl
kubectl edit svc/nginx-fixed-port

# Change the nodePort value under spec.ports[0].nodePort to another value (e.g., 30008)
```

### Exercise 5: Clean up resources

**Objective:** Delete all the resources created in the previous exercises.

**Steps:**
1. Delete the Deployments and Services related to Nginx.

**Solution:**
```bash
kubectl delete deployment nginx
kubectl delete svc nginx
kubectl delete svc nginx-fixed-port
```

