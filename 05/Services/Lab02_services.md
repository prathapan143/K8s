

Exercise 1:
Objective: Deploy a simple web application using NodePort service.
Solution:
1. Create a deployment for the web application.
2. Create a NodePort service to expose the deployment.
3. Access the web application using the NodePort service.

Exercise 2:
Objective: Scale up and down the deployment with NodePort service.
Solution:
1. Increase the replica count of the deployment.
2. Verify that new pods are created and the service is updated accordingly.
3. Decrease the replica count of the deployment.
4. Verify that excess pods are terminated and the service is updated accordingly.

Exercise 3:
Objective: Update the NodePort service to use a specific port.
Solution:
1. Edit the NodePort service and specify the desired port.
2. Verify that the service is updated and accessible on the new port.


Exercise 4:
Objective: Delete the NodePort service and clean up resources.
Solution:
1. Delete the NodePort service.
2. Verify that the service is no longer accessible.
3. Delete the deployment and associated resources.

Please note that these exercises are simplified examples and may require additional steps or configurations based on your specific environment and requirements.
You: Give me the solution for above lab exercises

AI: Sure! Here are the solutions for the lab exercises on Kubernetes NodePort:

Exercise 1 Solution:
1. Create a deployment for the web application:
`kubectl create deployment webapp --image=your-webapp-image`

2. Create a NodePort service to expose the deployment:
`kubectl expose deployment webapp --type=NodePort --port=80`

3. Access the web application using the NodePort service:
`kubectl get service webapp` (Note down the NodePort port number)
Open a web browser and enter: `http://<NodeIP>:<NodePort>;`

Exercise 2 Solution:
1. Increase the replica count of the deployment:
`kubectl scale deployment webapp --replicas=3`

2. Verify that new pods are created and the service is updated accordingly:
`kubectl get pods`
`kubectl get service webapp`

3. Decrease the replica count of the deployment:
`kubectl scale deployment webapp --replicas=1`

4. Verify that excess pods are terminated and the service is updated accordingly:
`kubectl get pods`
`kubectl get service webapp`

Exercise 3 Solution:
1. Edit the NodePort service and specify the desired port:
`kubectl edit service webapp`
Update the `spec.ports` section with the desired port number.

2. Verify that the service is updated and accessible on the new port:
`kubectl get service webapp`

Exercise 4 Solution:
1. Delete the NodePort service:
`kubectl delete service webapp`

2. Verify that the service is no longer accessible:
`kubectl get service webapp`

3. Delete the deployment and associated resources:
`kubectl delete deployment webapp`

Please note that these solutions assume you have a working Kubernetes cluster and the necessary permissions to perform these actions. Adjust the commands and configurations as needed for your specific environment.
