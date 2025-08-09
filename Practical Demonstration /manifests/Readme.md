## 🧪 A Practical Demonstration

After learning about Argo CD and its applications, let’s have a **hands-on demo** and see how it works.  
We will **install Argo CD in a Minikube cluster** and watch it in action.

---

### Step 1 — Start Minikube

If you don’t have Minikube installed, install it from the [official Minikube installation guide](https://minikube.sigs.k8s.io/docs/start/).  

**Command (copy & paste):**

```bash
minikube start
```

<img width="940" height="477" alt="image" src="https://github.com/user-attachments/assets/d5a08990-8e81-40e2-8e9c-2ea0cc5472d6" />

---

### Step 2 — Install Argo CD

We will now **install Argo CD** in the Kubernetes cluster.

**Commands (copy & paste):**

```bash
# Create the Argo CD namespace
kubectl create namespace argocd

# Install Argo CD using the official manifests
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

<img width="940" height="473" alt="image" src="https://github.com/user-attachments/assets/99a8914b-60fc-43b0-890b-c96158ad8eee" />

---

### Step 3 — Get the Initial Argo CD Admin Password

To log in to Argo CD, we first need the **initial admin password**.

**Command (copy & paste):**

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath='{.data.password}' | base64 -d
```

<img width="940" height="73" alt="image" src="https://github.com/user-attachments/assets/1e69acbb-c511-438c-93b2-206905880abb" />

---

### Step 4 — Expose the Argo CD API Server

To access the Argo CD UI locally, we will **expose the Argo CD API server** using port forwarding.

**Command (copy & paste):**

```bash
kubectl port-forward svc/argocd-server -n argocd 8081:443
```

<img width="940" height="91" alt="image" src="https://github.com/user-attachments/assets/7e24e130-8fb6-417d-9c0c-c74b6d87ded6" />

---

### Step 5 — Access the Argo CD UI

Now, open your browser and go to:

**[https://localhost:8081](https://localhost:8081)**

You will likely see a **security warning** because Argo CD uses a self-signed certificate.  

**Steps:**
1. Accept the security warning.
2. Confirm that you want to proceed to the site.
3. The Argo CD login page should appear.

**Important:**  
- **Accept the risk and continue** — this is safe in a local demo environment.  
- For production environments, configure a valid TLS certificate.

<img width="940" height="419" alt="image" src="https://github.com/user-attachments/assets/bdb28504-86ac-4074-b5b8-0444156fd157" />

---

### Step 6 — Log in to Argo CD

On the Argo CD login page:

- **Username:** `admin`  
- **Password:** The value you saved from **Step 3**.

**Important:**  
- **Do not share this password** — it grants full admin access to Argo CD.  
- Once logged in, you will be taken to the Argo CD dashboard.

<img width="907" height="438" alt="image" src="https://github.com/user-attachments/assets/8f9f5a6a-02eb-4344-b062-c821a7ace187" />

---

### Step 7 — Access the Dashboard & Create an Application

After entering your credentials, you will be presented with the **Argo CD Dashboard**.  

From here:  
1. Click on **Create Application** in the top navigation bar or main panel.  
2. This will open the application creation form where you can configure your deployment.

<img width="940" height="450" alt="image" src="https://github.com/user-attachments/assets/e7b7db6e-d621-414a-ba3d-40883d8740b9" />

---

### Step 8 — Configure and Create the Application

Fill in the application creation form with the following details:

**General Settings:**
- **Application Name:** `demo`
- **Project Name:** `default`
- **Sync Policy:** `Automatic`

**Source:**
- **Repository URL:** `https://github.com/k4kamil/ArgoCD.git`
- **Revision:** `HEAD`
- **Path:** `Practical Demonstration /manifests`

**Destination:**
- **Cluster URL:** `https://kubernetes.default.svc`
- **Namespace:** `default`

Once all fields are filled:
1. Click **Create**.
2. Wait **2–3 minutes** for the application resources to be deployed and for all pods to become healthy.

**Important:**  
- **Ensure that your repository is public** or that Argo CD has the required authentication to access it.
- Application health status will be shown in the dashboard once the sync completes.

<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/ec06bc71-ede4-4f8e-be30-20d0e24edd5b" />

<img width="940" height="449" alt="image" src="https://github.com/user-attachments/assets/8af89041-1c14-400f-9b88-37ed92d28a71" />

---

### Step 9 — Test GitOps with Scaling Changes

Now we will see Argo CD's **GitOps magic** in action by making changes both from the cluster and from the Git repository.

#### 1️⃣ Scale from the Cluster
Run the following command to change the number of replicas of `nginx-deployment`:

```bash
kubectl scale --replicas=2 deployment/nginx-deployment
```

💡 What happens:
  The number of pods will change to 2.


<img width="940" height="448" alt="image" src="https://github.com/user-attachments/assets/11088324-393d-4c02-9226-4f1868f0bce1" />

---

### Step 10 — Clean Up Resources

Once you are done with the demo, you can clean up the resources by deleting the Minikube cluster:

```bash
minikube delete
```
---
