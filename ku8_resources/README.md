````markdown
# Working with Kubernetes Resources

This hands-on project demonstrates how to deploy and expose an Nginx web application on Minikube using Kubernetes YAML configuration files.

---

## 1. Introduction to YAML

A **Kubernetes YAML file** is a text file written in YAML syntax that describes and defines Kubernetes resources such as Pods, Containers, Services, and Deployments.  
YAML uses indentation to represent hierarchy and spaces (not tabs) for indentation. In Kubernetes, YAML provides a declarative way to specify the desired state of resources.

![github](img/yaml-introduction.png)

---

## 2. Basic Structure of a YAML File

- **Scalars**: Single values such as strings, numbers, booleans.
- **Collections**:
  - Lists (arrays)
  - Maps (key-value pairs)
- **Nested Structures**: Allows hierarchy.
- **Comments**: Start with `#`.
- **Multiline Strings**: Represented with `|` or `>`.
- **Anchors & Aliases**: `&` to create an anchor, `*` to use it.

![github](img/yaml-structure.png)

---

## 3. Deploying Applications in Kubernetes

Deployments in Kubernetes define the desired state of an application, including scaling and update strategies.  
Services provide a stable endpoint for accessing Pods.

Types of Services:
- **ClusterIP** – Accessible only inside the cluster.
- **NodePort** – Accessible externally on a static port.
- **LoadBalancer** – Uses cloud provider load balancer for external access.

![github](img/kubernetes-services.png)

---

## 4. Start Minikube

```bash
minikube start
````

![github](img/start-minikube.png)

---

## 5. Create Project Directory

```bash
mkdir my-nginx-yaml
cd my-nginx-yaml
```

![github](img/create-folder.png)

---

## 6. Create Deployment YAML

**File:** `nginx-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
    spec:
      containers:
        - name: my-nginx
          image: <your-dockerhub-username>/my-nginx:1.0
          ports:
            - containerPort: 80
```

> Replace `<your-dockerhub-username>` with your Docker Hub username.

![github](img/create-deployment-yaml.png)

---

## 7. Create Service YAML

**File:** `nginx-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-service
spec:
  selector:
    app: my-nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

![github](img/create-service-yaml.png)

---

## 8. Apply Deployment

```bash
kubectl apply -f nginx-deployment.yaml
```

![github](img/apply-deployment.png)

---

## 9. Apply Service

```bash
kubectl apply -f nginx-service.yaml
```

![github](img/apply-service.png)

---

## 10. Verify Resources

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

![github](img/verify-resources.png)

---

## 11. Access Application

```bash
minikube service my-nginx-service
```

This opens the Nginx welcome page in a browser.

![github](img/nginx-browser.png)

---

## 12. Push Project to GitHub

```bash
git init
git add .
git commit -m "Kubernetes Nginx Deployment and Service"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

![github](img/push-to-git.png)

---

## Project Structure

```
my-nginx-yaml/
│
├── nginx-deployment.yaml
├── nginx-service.yaml
├── img/
│   ├── yaml-introduction.png
│   ├── yaml-structure.png
│   ├── kubernetes-services.png
│   ├── start-minikube.png
│   ├── create-folder.png
│   ├── create-deployment-yaml.png
│   ├── create-service-yaml.png
│   ├── apply-deployment.png
│   ├── apply-service.png
│   ├── verify-resources.png
│   ├── nginx-browser.png
│   └── push-to-git.png
└── README.md
```
