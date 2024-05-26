**Setting Up MongoDB on Kubernetes with Minikube**

MongoDB is a popular NoSQL database known for its flexibility and scalability, and Kubernetes is a powerful container orchestration tool that automates the deployment, scaling, and management of containerized applications.

### Prerequisites

1. **Minikube**: Ensure Minikube is installed on your local machine. Minikube provides a single-node Kubernetes cluster for development and testing purposes.
2. **kubectl**: Install the Kubernetes command-line tool `kubectl` to interact with your Kubernetes cluster.

Run docker on another terminal in WSL using:

```bash
sudo dockerd
```

### Step 1: Start Minikube

Open a terminal and start Minikube by running the following command:

```bash
minikube start
```

### Step 2: Create MongoDB Deployment

Create a MongoDB deployment by creating a YAML file named `mongodb-deployment.yaml` with the following contents:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo
          ports:
            - containerPort: 27017
```

Apply the deployment configuration using the following command:

```bash
kubectl apply -f mongodb-deployment.yaml
```

### Step 3: Create MongoDB Service

Create a MongoDB service to expose the MongoDB deployment within the Kubernetes cluster. Create a YAML file named `mongodb-service.yaml` with the following contents:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongodb
spec:
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
      targetPort: 27017
```

Apply the service configuration using the following command:

```bash
kubectl apply -f mongodb-service.yaml
```

### Step 4: Verify Deployment

Verify that the MongoDB deployment and service are running correctly by checking the pods and services:

```bash
kubectl get pods
kubectl get services
```

### Step 5: Set Up Debugging Pod

Create a debugging pod to interact with the MongoDB deployment. Create a YAML file named `debugging-pod.yaml` with the following contents:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mongo-debugger
spec:
  containers:
  - name: mongo-client
    image: mongo
    command: ["/bin/sleep", "3600"]  # Sleep indefinitely to keep the pod running
    stdin: true
    tty: true
```

Apply the debugging pod configuration using the following command:

```bash
kubectl apply -f debugging-pod.yaml
```

### Step 6: Connect to MongoDB

Forward the MongoDB service port to your local machine:

```bash
kubectl port-forward service/mongodb 27017:27017
```

Install MongoDB client tools on your local machine:

```bash
sudo apt install mongodb-clients
```

Connect to the MongoDB deployment using the MongoDB shell:

```bash
mongo --host 127.0.0.1 --port 27017
```

Once connected, you can interact with the MongoDB deployment using MongoDB commands. For example, you can insert data into the `users` collection and query the data:

```javascript
db.users.insertOne({ name: "John", age: 30 });
db.users.find();
```
