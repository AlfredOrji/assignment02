# Python Web Application Deployment

This repository contains a Python web application that displays the current time in Toronto, Canada. The application is containerized using Docker and deployed on Kubernetes.
Instructions
Build and Run the Docker Container

1. Build the Docker Image:
    bash
    docker build -t alfredeorji762/project2 .
    

2. Push the Docker Image to Docker Hub:
    bash
    docker login
    docker push alfredeorji762/project2:tagname
    

3. Run the Docker Container Locally:
    bash
    docker run --network p1 --name app -dp 3030:3030 alfredeorji762/project2:tagname
   

4. Access the Application:
    Open a web browser and go to http://localhost:3030.

### Deploy and Access the Application on Kubernetes

1. *Create Deployment YAML File* (deployment.yaml):
    yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: python-app-deployment
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: python-app
      template:
        metadata:
          labels:
            app: python-app
        spec:
          containers:
          - name: python-app
            image: alfredeorji762/project2 .
            ports:
            - containerPort: 3030
    

2. *Create Service YAML File* (service.yaml):
    yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: python-app-service
    spec:
      type: NodePort
      selector:
        app: python-app
      ports:
        - protocol: TCP
          port: 3030
          targetPort: 3030
          nodePort: 30330
    

3. Ensure Kubernetes Cluster is Running:
    bash
    minikube start
    

4. Apply Deployment and Service:
    bash
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    

5. Access the Application:
    bash
    minikube ip
    
    Open a web browser and go to http://<node-ip>:30330.

Challenges and Solutions

1. Docker Image Build Issues:
   Challenge: Missing Dockerfile.
   Solution: Created Dockerfile with correct instructions.

2. Unauthorized Docker Push:
   Challenge: "Access token has insufficient scopes" error.
   Solution: Logged out and back in using docker logout and docker login.

3. Kubernetes Connection Issues:
   Challenge: kubectl failed to connect to API server.
   Solution: Ensured Kubernetes cluster (Minikube) was running and configured kubectl correctly.

4. NodePort Range Issue:
   Challenge: Specified nodePort (3030) outside valid range.
   Solution: Updated nodePort to 30330 within the valid range.

5. Accessing Application:
   Challenge: Incorrect Node IP or port.
   Solution: Verified Node IP with minikube ip and ensured correct NodePort.
