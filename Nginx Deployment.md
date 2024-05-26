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

Scale the deployment to 4 replicas:

```bash
kubectl scale deployment my-nginx --replicas=4
```

### Step 7: Expose Deployment

Expose the deployment to make it accessible via a NodePort:

```bash
kubectl expose deployment my-nginx --type=NodePort --port=80
```

### Step 8: Access Service

Get the URL to access the Nginx service:

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

Delete the deployment to clean up resources:

```bash
kubectl delete deployment my-nginx
```

This will delete the deployment.

### Different Service Types

1. **ClusterIP (default)**:
- **Description**: Exposes the service on an internal IP in the cluster. This type makes the service only accessible within the cluster.
- **Use Case**: Internal communication between different services within the cluster.
- **Command Example**:
  
```bash
kubectl expose deployment my-nginx --type=ClusterIP --port=80
```

2. **NodePort**:
- **Description**: Exposes the service on each Node's IP at a static port (the NodePort).
- A `ClusterIP` service, to which the `NodePort` service routes, is automatically created.
- **Use Case**: Makes the service accessible from outside the cluster by requesting `<NodeIP>:<NodePort>`.
- **Command Example**:
  
```bash
kubectl expose deployment my-nginx --type=NodePort --port=80
```

3. **LoadBalancer**:
- **Description**: Exposes the service externally using a cloud provider's load balancer. The cloud provider automatically creates an external load balancer that routes to your service.
- **Use Case**: Suitable for production environments where you need to expose your service to the internet with a public IP.
- **Command Example**:
  
```bash
kubectl expose deployment my-nginx --type=LoadBalancer --port=80
```

4. **ExternalName**:
- **Description**: Maps the service to the contents of the `externalName` field (e.g., `foo.bar.example.com`) by returning a CNAME record.
- **Use Case**: Used for exposing services that are external to the cluster by returning a CNAME record with the external name.
- **Command Example**:
  
```bash
kubectl create service externalname my-nginx --external-name=example.com
```

#### Summary of Service Types

- **ClusterIP**: Internal access only.
- **NodePort**: Exposes service on each node's IP at a static port.
- **LoadBalancer**: Uses cloud provider's load balancer to expose the service externally.
- **ExternalName**: Maps service to an external name.
