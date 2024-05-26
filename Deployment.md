# Kubernetes Deployment with Nginx

- `kubectl` is a command-line tool used for interacting with Kubernetes clusters. It allows users to deploy and manage applications, inspect and manage cluster resources, and view logs and other diagnostic information.

- `minikube` is a tool that enables users to run Kubernetes clusters locally on their machines. It is primarily used for development, testing, and learning purposes, providing an easy way to set up a single-node Kubernetes cluster without requiring a full-scale infrastructure.

## Tools Installation

### Install kubectl

Kubectl is the command-line tool for interacting with Kubernetes clusters. You can install it using the following commands:

```bash
curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```

### Install Minikube

Minikube is a tool to run Kubernetes locally. To install Minikube, run:

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

## Setting Up Kubernetes Deployment

### Start Minikube

Start Minikube and Docker daemon in separate terminals:

```bash
# Terminal-1
sudo dockerd

# Terminal-2
minikube start
```

### Create Deployment YAML

Create a Kubernetes deployment YAML file for Nginx. You can create a file named `nginx-deployment.yaml` with the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Apply the Deployment

Apply the deployment YAML to create the Nginx deployment:

```bash
kubectl apply -f nginx-deployment.yaml
```

### Expose the Deployment

Expose the Nginx deployment by creating a service. You can expose it as a NodePort service for accessing it from localhost. Create a file named `nginx-service.yaml` with the following content:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

Apply the service:

```bash
kubectl apply -f nginx-service.yaml
```

### Verify Deployment

Ensure that the Nginx pod is running and healthy:

```bash
kubectl get pods
```

Get the Minikube IP:

```bash
minikube ip
```

Get the service:

```bash
kubectl get svc nginx-service
```

Forward the port to access Nginx:

```bash
kubectl port-forward <nginx_pod_name> 8080:80
```

Then, try accessing Nginx using [http://localhost:8080](http://localhost:8080) in your browser.

The minikube dashboard command launches the Kubernetes dashboard for the Minikube cluster. This dashboard provides a web-based user interface for managing and monitoring your Minikube Kubernetes cluster.

```bash
minikube dashboard
```

## Conclusion

You've successfully deployed Nginx on a Kubernetes cluster using Minikube. 
