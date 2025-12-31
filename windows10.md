# 🪟 Windows 10 → Senior DevOps/SRE Lab (FREE)

![Image](https://www.thomasmaurer.ch/wp-content/uploads/2019/06/WSL-2-Architecture.jpg)

![Image](https://kind.sigs.k8s.io/docs/images/diagram.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1200/1%2AGgIeRA40tqhLPUSpJ1kMzw.png)

## ✅ High-level approach (important)

> **Windows 10 + WSL2 (Ubuntu) + Docker Desktop + Kind**
> You will work **inside Linux**, but control everything from Windows.

This gives you:

* Real Linux environment
* Kubernetes realism
* Zero AWS bills
* Interview-ready experience

---

# 1️⃣ Prerequisites (One-time setup)

## 🔹 Step 1: Enable WSL2 (MANDATORY)

Open **PowerShell as Administrator**:

```powershell
wsl --install
```

Reboot when asked.

Set WSL2 as default:

```powershell
wsl --set-default-version 2
```

Install **Ubuntu 22.04** from Microsoft Store.

---

## 🔹 Step 2: Install Docker Desktop (FREE)

Download:
👉 [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

During install:

* ✅ Enable **WSL2 backend**
* ✅ Enable **Ubuntu integration**

After install:

* Open Docker Desktop
* Settings → Resources → WSL Integration → Enable Ubuntu

Verify inside Ubuntu:

```bash
docker run hello-world
```

---

# 2️⃣ Linux Toolchain (Inside Ubuntu)

Open **Ubuntu (WSL)** and install:

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y \
  curl wget git unzip \
  ca-certificates gnupg \
  software-properties-common
```

---

## 🔹 Install kubectl

```bash
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

---

## 🔹 Install Kind (Kubernetes)

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
chmod +x kind
sudo mv kind /usr/local/bin/
```

---

## 🔹 Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

---

## 🔹 Install Terraform

```bash
wget https://releases.hashicorp.com/terraform/1.6.6/terraform_1.6.6_linux_amd64.zip
unzip terraform_1.6.6_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

Verify:

```bash
docker --version
kubectl version --client
kind version
helm version
terraform version
```

---

# 3️⃣ Create Your Kubernetes Cluster (Multi-Node)

This is **VERY IMPORTANT** for senior-level realism.

```bash
cat <<EOF > kind.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF
```

Create cluster:

```bash
kind create cluster --config kind.yaml
```

Check:

```bash
kubectl get nodes
```

You now have:
✔ Node failures
✔ Pod rescheduling
✔ Network debugging
✔ Storage issues

---

# 4️⃣ GitHub (FREE CI/CD)

Create repo:

```
devops-sre-lab
```

Recommended structure:

```text
devops-sre-lab/
├── docker/
├── k8s/
├── helm/
├── terraform/
├── argocd/
├── observability/
├── incidents/
└── diagrams/
```

Enable **GitHub Actions** (default).

---

# 5️⃣ CI/CD – GitHub Actions (FREE)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1200/1%2ABqhBVtvV8233x6WdlaLOeQ.png)

![Image](https://d2908q01vomqb2.cloudfront.net/22d200f8670dbdb3e253a90eee5098477c95c23d/2020/06/26/CICD-Container-Scannning-Figure-1s.png)

What you’ll build:

* Build Docker image
* Scan with Trivy
* Push image
* Trigger GitOps deploy

This alone answers **10+ senior interview questions**.

---

# 6️⃣ GitOps – Argo CD (Local)

Install:

```bash
kubectl create namespace argocd

kubectl apply -n argocd \
-f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Expose UI:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access:
👉 [https://localhost:8080](https://localhost:8080)

✔ Declarative deploys
✔ Rollbacks
✔ Drift detection

---

# 7️⃣ Observability Stack (FREE)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2ABFUh0zgr8Lp2Rlkd5XpQdw.png)

![Image](https://miro.medium.com/1%2AFRMJBWkx0vcGPpu8rIIzaA.png)

![Image](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2A6toKWw-IWsJh7gYaVayN6A.png)

Install monitoring:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

helm install monitoring prometheus-community/kube-prometheus-stack
```

You’ll practice:

* SLIs / SLOs
* Alert tuning
* Incident debugging

---

# 8️⃣ Chaos & Incident Practice (FREE)

Examples:

```bash
kubectl delete pod app-pod
kubectl drain kind-worker
kubectl scale deployment app --replicas=0
```

Add CPU pressure:

```bash
kubectl run stress --image=polinux/stress -- stress --cpu 2
```

✔ SEV-1 simulation
✔ Postmortems

---

# 9️⃣ Optional: OpenShift on Windows (If Targeting OCP)

![Image](https://www.devopsschool.com/blog/wp-content/uploads/2025/05/image-29.png)

![Image](https://access.redhat.com/webassets/avalon/d/Red_Hat_OpenShift_Container_Storage-4.8-Planning_your_deployment-en-US/images/d84f73571933a2d30858f6a7daa623ce/OCS_SPA_Architecture.png)

Use **CodeReady Containers (CRC)**:

* Requires ~16GB RAM
* Good for **OpenShift Admin roles**

---

# 🔥 What This Lab Covers (Interview Mapping)

| Topic               | Covered |
| ------------------- | ------- |
| Kubernetes failures | ✅       |
| GitOps              | ✅       |
| CI/CD               | ✅       |
| Observability       | ✅       |
| SRE incidents       | ✅       |
| Terraform           | ✅       |
| Cloud concepts      | ✅       |

---

# ⚠️ Windows-Specific Mistakes to Avoid

❌ Working directly in PowerShell
❌ Running minikube instead of Kind
❌ Paying for AWS too early
❌ No documentation

✔ **Do everything inside Ubuntu (WSL2)**

---
