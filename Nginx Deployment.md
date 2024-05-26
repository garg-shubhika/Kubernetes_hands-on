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

## Getting Started

### Step 1: Start Docker in terminal-1

```bash
sudo dockerd
```

### Step 2: Start Minikube in terminal-2

```bash
minikube start
```

### Step 3: Create a Deployment

```bash
kubectl create deployment my-nginx --image=nginx
```

### Step 4: Verify Deployment

```bash
kubectl get deployment
```
In the output of kubectl get deployment:

- READY: Shows the number of pods that are ready out of the desired number of pods. In this case, 4/4 means all 4 desired pods are running and ready.
- UP-TO-DATE: The number of pods that have been updated to achieve the desired state.
- AVAILABLE: The number of pods that are available to serve requests.
- AGE: How long the deployment has been running for.

### Step 5: Get Pods

```bash
kubectl get pods
```

You should see the status of each pod.

- READY: Shows how many containers in the pod are ready (e.g., 1/1 means one container is ready).
- STATUS: The status of the pod, which should be Running for active pods.
- RESTARTS: The number of times the pod has been restarted.
- AGE: How long the pod has been running.

  
### Step 6: Scale Deployment

```bash
kubectl scale deployment my-nginx --replicas=4
```

### Step 7: Expose Deployment

```bash
kubectl expose deployment my-nginx --type=NodePort --port=80
```

### Step 8: Access Service

```bash
minikube service my-nginx --url
```

Copy the URL and paste it into your browser.

### Step 9: Access Minikube Dashboard

```bash
minikube dashboard
```

This will open up a web interface where you can view your deployment, pods, and replica sets.

### Step 10: Get Minikube IP

```bash
minikube ip
```

This will give you the IP address of your Minikube cluster.

### Step 11: Clean Up

```bash
kubectl delete deployment my-nginx
```

This will delete the deployment.
