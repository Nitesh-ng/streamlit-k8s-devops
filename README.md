# ☸️ Kubernetes-Based Streamlit App Deployment with Docker & AWS

## 📘 Overview
This project demonstrates the deployment of a **Streamlit-based Python web application** using **Docker**, **Kubernetes (K3s)**, and **AWS EC2**.  
The application was containerized, pushed to **Docker Hub**, deployed on a **K3s Kubernetes cluster**, and made accessible through a **custom domain** using **AWS Route 53**.  

---

## 🧰 Tools & Technologies Used
- **Docker** → Containerization of the Streamlit application  
- **Kubernetes (K3s)** → Lightweight cluster setup for orchestration  
- **AWS EC2** → Hosting for Kubernetes nodes  
- **AWS Route 53** → Domain management and DNS-based routing  
- **Docker Hub** → Image registry for storing application images  
- **GitHub** → Version control and project collaboration  

---

## 🏗️ Project Stages
| Stage | Description |
|--------|--------------|
| 🧱 **Dockerization** | Created a Dockerfile to containerize the Streamlit app. |
| ☸️ **Kubernetes Deployment** | Defined Deployment and NodePort Service YAMLs to run the app in K3s. |
| ☁️ **AWS Setup** | Launched an EC2 instance and configured inbound rules for SSH and app access. |
| 🌐 **Domain Integration** | Connected a custom subdomain using AWS Route 53 DNS routing. |
| 🔍 **Verification** | Tested app accessibility via EC2 Public IP and custom domain URL. |
| 🧹 **Teardown** | Terminated resources safely to avoid AWS charges. |

---
## 🪜 Step-by-Step Setup

````markdown

### 1️⃣ Launch EC2 Instance
- Instance type: **t3.small (2 vCPU, 2GB RAM)**  
- OS: **Ubuntu 22.04 LTS**  
- Inbound rules:  
  - **22** (SSH)  
  - **80** (HTTP)  
  - **31000** (NodePort)  

SSH into instance:
ssh -i "k8s-ec2-node.pem" ubuntu@<EC2-Public-IP>
````

---

### 2️⃣ Install Docker & K3s

```bash
sudo apt update -y
sudo apt install -y docker.io
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
```

---

### 3️⃣ Clone Repository

```bash
git clone https://github.com/Nitesh-ng/streamlit-k8s-devops.git
cd streamlit-k8s-devops
```

---

### 4️⃣ Build & Push Docker Image

```bash
docker build -t nitesh181/streamlit-marathi-summary:latest .
docker push nitesh181/streamlit-marathi-summary:latest
```

---

### 5️⃣ Deploy to Kubernetes

Apply the deployment and service files:

```bash
kubectl apply -f streamlit-deploy.yaml
kubectl get pods -o wide
kubectl get svc -o wide
```

Access your application at:

```
http://<EC2-Public-IP>:31000
```

---

### 6️⃣ Configure AWS Route 53

1. Create a hosted zone → `niteshgaikwad.online`
2. Add a record:

   * Type: **A Record**
   * Name: `marathi-summary.niteshgaikwad.online`
   * Value: **Elastic IP of EC2 Instance**
3. Wait for DNS propagation (~30–60 mins)
4. Verify:

```bash
nslookup marathi-summary.niteshgaikwad.online
```

Now access:

```
http://marathi-summary.niteshgaikwad.online:31000
```

---

## ✅ Project Verification Steps

1. **Docker Image pushed to Docker Hub**

   * Confirmed using: `docker images` and Docker Hub UI.
2. **Kubernetes Deployment active**

   * Verified pods running: `kubectl get pods -o wide`
3. **Service Exposed via NodePort**

   * Verified with: `kubectl get svc`
4. **Custom Domain Working**

   * Accessed successfully using Route 53 subdomain and EC2 IP.
5. **Teardown Validation**

   * Confirmed all AWS resources terminated after completion to avoid billing.

---

## 🖼️ Project Screenshots

1. EC2 Instance Dashboard (show instance type & IP)
   <img width="1920" height="1080" alt="Screenshot (236)" src="https://github.com/user-attachments/assets/b9fc782d-026b-455e-a34c-9046d83ec59b" />

3. Docker Hub repository (show image pushed)
  <img width="1920" height="1080" alt="Screenshot (239)" src="https://github.com/user-attachments/assets/eaca38c5-c169-47ee-b67d-fd2da8f7022c" />

4. Kubernetes terminal outputs (`kubectl get pods` & `kubectl get svc`)
  <img width="1920" height="1080" alt="Screenshot (240)" src="https://github.com/user-attachments/assets/3d5b2480-8f53-4dae-950b-6ddb08aeb24c" />
  <img width="1919" height="493" alt="Screenshot 2025-11-06 050212" src="https://github.com/user-attachments/assets/8188ee0c-edef-4ec5-a02f-ab2bf201feed" />

5. Browser view of Streamlit app running
  <img width="1920" height="1080" alt="Screenshot (230)" src="https://github.com/user-attachments/assets/e703ce77-5d7f-425a-9a04-932f8d60ba00" />

6. AWS Route 53 hosted zone and record setup
  <img width="1920" height="1080" alt="Screenshot (227)" src="https://github.com/user-attachments/assets/ddb59ad0-4daa-4650-b957-f50448048004" />

---

## 🧩 Conclusion

This project demonstrates a **complete AWS DevOps pipeline** for deploying a Python web app using **Docker, Kubernetes (K3s)**, and **AWS infrastructure**.
It includes real-world practices such as containerization, domain configuration, and Kubernetes orchestration.

🎯 **Result:**
A fully deployed, containerized **Streamlit web app** accessible publicly via a **custom Route 53 domain** — built, deployed, and managed by AWS DevOps practices.
