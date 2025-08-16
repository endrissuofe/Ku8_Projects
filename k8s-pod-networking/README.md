
# Networking in Kubernetes – Pod Networking Demo

This project demonstrates **Pod Networking in Kubernetes** by deploying a pod with two containers:

* An **Nginx container** serving a web page.
* A **BusyBox container** continuously writing text into the Nginx HTML file.

Both containers share the **same network namespace**, meaning they can communicate with each other using **localhost**.

---

## 📖 Concept Explanation

Kubernetes networking provides mechanisms for communication between **pods, services, and external clients**. Some key aspects include:

* **Pod Networking**: Containers inside the same pod share the same network namespace and communicate via `localhost`.
* **Service Networking**: Services expose a group of pods through a stable network endpoint (ClusterIP, NodePort, or LoadBalancer).
* **Pod-to-Pod Communication**: Kubernetes assigns each pod its own IP, allowing pods across nodes to talk directly through an overlay network.
* **Ingress**: Provides HTTP/HTTPS routing rules to allow external traffic into the cluster.
* **Network Policies**: Define rules to control which pods can communicate, improving security.
* **Container Network Interface (CNI)**: Plugins that implement networking in Kubernetes, such as Flannel, Calico, or Weave.

In this project, we focus on **Pod Networking**, showing how containers inside the same pod share networking and can interact seamlessly.

---

## 📌 Prerequisites

Before starting, ensure you have the following installed:

* [Docker](https://docs.docker.com/get-docker/)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

Verify installations:

```bash
docker --version
minikube version
kubectl version --client
```

📸 Screenshot: verification of installed tools
![verify-tools](img/verify-tools.png)

---

## 🚀 Step 1: Project Setup

Create a project directory and an `img` folder for screenshots.

```bash
mkdir -p k8s-pod-networking/img
cd k8s-pod-networking
```

📸 Screenshot: terminal after creating folder

![project-setup](img/project-setup.png)

---

## 🚀 Step 2: Create Pod Manifest

Create a file `multi-container-pod.yaml` with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: container-1
    image: nginx:alpine
    ports:
    - containerPort: 80
  - name: container-2
    image: busybox
    command: ['sh', '-c', 'while true; do echo "Hello from Container 2" >> /usr/share/nginx/html/index.html; sleep 10; done']
```

📸 Screenshot: YAML file in editor

![yaml-file](img/yaml-file.png)

---

## 🚀 Step 3: Deploy the Pod

Apply the manifest:

```bash
kubectl apply -f multi-container-pod.yaml
```

📸 Screenshot: output of `kubectl apply`

```markdown
![kubectl-apply](img/kubectl-apply.png)
```

---

## 🚀 Step 4: Verify Pod Status

Check the pod status:

```bash
kubectl get pods
kubectl describe pod multi-container-pod
```

📸 Screenshot: output of `kubectl get pods`

![kubectl-get-pods](img/kubectl-get-pods.png)

---

## 🚀 Step 5: Access the Nginx Container

Forward port **8080 → 80** so you can access the Nginx container in your browser.

```bash
kubectl port-forward pod/multi-container-pod 8080:80
```

📸 Screenshot: terminal showing port-forward running

![port-forward](img/port-forward.png)

Now, open a browser and visit:

```
http://localhost:8080
```

📸 Screenshot: Nginx page in browser

![nginx-browser](img/nginx-browser.png)

---

## 🚀 Step 6: Test Communication from BusyBox

Open a shell inside the BusyBox container:

```bash
kubectl exec -it multi-container-pod -c container-2 -- sh
```

Inside BusyBox, run:

```sh
wget -qO- http://localhost
```

You should see the message `"Hello from Container 2"` appended repeatedly.

📸 Screenshot: BusyBox terminal showing wget output

![busybox-curl](img/busybox-curl.png)


---

## 🚀 Step 7: Clean Up

Delete the pod when done:

```bash
kubectl delete -f multi-container-pod.yaml
```

📸 Screenshot: cleanup confirmation

![kubectl-delete](img/kubectl-delete.png)

---

## 📖 Observations

* Both containers share the same **network namespace**, meaning they can reach each other via `localhost`.
* The **Nginx container** serves the HTML page.
* The **BusyBox container** continuously updates the page content.
* This demonstrates how **multi-container pods** achieve tight coupling through shared networking.

---
## ✅ Conclusion

In this project, we saw how Kubernetes Pod Networking works in practice.

* Containers inside the same pod can communicate with each other directly using localhost.
* This makes it possible for tightly coupled workloads (like sidecar containers) to share resources efficiently.
* Pod Networking is the foundation for higher-level networking concepts in Kubernetes such as Services, Ingress, and Network Policies.
