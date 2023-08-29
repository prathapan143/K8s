Sure! Here's your content in markdown format:

```markdown
# Nginx Ingress Controller Setup

The Nginx Ingress Controller has been installed and an Ingress resource configured in Namespace `world`.

## Accessing the application

You can reach the application using:

```
curl http://world.universe.mine:30080/europe
```

## Generating a new TLS certificate

Generate a new certificate with:

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt -subj "/CN=world.universe.mine/O=world.universe.mine"
```

After generating the certificate, configure the Ingress to use the new certificate. After this, you should be able to call:

```
curl -kv https://world.universe.mine:30443/europe
```

The curl verbose output should show the new certificate being used instead of the default Ingress one.

## Tips

1. **Creating a Secret**:
   
   ```
   k -n world create secret tls -h
   ```
   
   Then make the Ingress resource use it:
   
   ```
   k -n world edit ing world
   ```

2. **Generating crt and key**:

   ```
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.crt -subj "/CN=world.universe.mine/O=world.universe.mine"
   ```

3. **Creating a Secret from the generated files**:
   
   ```
   kubectl -n world create secret tls ingress-tls --key cert.key --cert cert.crt
   ```

4. **Configuring Ingress to use the Secret**:

   ```
   k -n world edit ing world
   ```

## Ingress Configuration

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: world
  namespace: world
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  ingressClassName: nginx
  tls:                            
  - hosts:                        
    - world.universe.mine         
    secretName: ingress-tls       
  rules:
  - host: "world.universe.mine"
    http:
      paths:
      - path: /europe
        pathType: Prefix
        backend:
          service:
            name: europe
            port:
              number: 80
      - path: /asia
        pathType: Prefix
        backend:
          service:
            name: asia
            port:
              number: 80
```

## Verifying the TLS certificate

To check that the right certificate is being used:

```
curl -m1 -kvI https://world.universe.mine:30443/europe 2>&1 | grep subject | grep world.universe.mine
```
```

You can copy the markdown content and use it in markdown-supported platforms.
