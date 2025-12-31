
This will give you a **realistic, almost-zero-cost lab setup** that supports (Kubernetes, Terraform, GitOps, CI/CD, observability, SRE, incidents) and is **safe for daily break–fix practice**.

This will give you **one primary setup** and **two optional add-ons**, then a **step-by-step Day-0 checklist**.

---

# 🔥 Recommended Free / Near-Free Lab Architecture (Senior-Level)

![Image](https://fernandocejas.com/assets/img/blog/over_engineered_home_lab_docker_kubernetes_kubernetes_approach.png)

![Image](https://drek4537l1klr.cloudfront.net/luksa3/v-5/Figures/3.4.png)

![Image](https://argo-cd.readthedocs.io/en/stable/assets/argocd_architecture.png)

![Image](https://miro.medium.com/1%2AbjKiyBXYGZ3W3YITATDBvA.png)

## Core idea

> **Local Kubernetes (Kind) + Free GitHub + Free SaaS tools**
> This avoids AWS bills while still teaching **production concepts**.

---

## 1️⃣ Base Layer (100% Free)

### 🧠 Your Laptop (Primary Control Plane)

* OS: Linux / macOS / Windows (WSL2)
* RAM: 8–16 GB recommended
* CPU: 4 cores+

### Tools to install

| Tool      | Why               |
| --------- | ----------------- |
| Docker    | Containers        |
| Kind      | Local Kubernetes  |
| kubectl   | Cluster control   |
| Helm      | App packaging     |
| Terraform | IaC               |
| Git       | Version control   |
| k9s       | Runtime debugging |

---

## 2️⃣ Kubernetes Cluster (Kind)

### Create multi-node cluster (important for realism)

```bash
cat <<EOF > kind.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

kind create cluster --config kind.yaml
```

✔ Node failure testing
✔ Pod rescheduling
✔ Network issues
✔ Storage issues

---

## 3️⃣ GitHub (Free Tier)

### Use **GitHub Free**

* Unlimited public repos
* GitHub Actions (free minutes)
* Native secrets

### Repo structure (recommended)

```text
devops-lab/
├── terraform/
├── k8s-manifests/
├── helm-charts/
├── argocd/
├── observability/
├── incidents/
└── diagrams/
```

---

## 4️⃣ CI/CD – GitHub Actions (Free)

![Image](https://d2908q01vomqb2.cloudfront.net/7719a1c782a1ba91c031a682a0a2f8658209adbf/2022/03/27/1-ArchitectureDiagram.png)

![Image](https://devopscube.com/content/images/2025/03/trivy-blog-image-1.gif)

![Image](https://codefresh.io/wp-content/uploads/2023/06/Basic-GitOps-workflow-for-Kubernetes.png)

### What you’ll build

* PR checks
* Docker build
* Trivy security scan
* Push image
* GitOps sync trigger

### Why this is senior-level

* Same pattern used in real companies
* Interview gold

---

## 5️⃣ GitOps – Argo CD (Free, Local)

```bash
kubectl create ns argocd
kubectl apply -n argocd \
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Expose UI:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

✔ Declarative deployments
✔ Rollbacks
✔ Drift detection

---

## 6️⃣ Observability Stack (All Free)

![Image](https://cdn.prod.website-files.com/681e366f54a6e3ce87159ca4/6877c683c0a47068f5e6609c_Blog-Kubernetes-Monitoring-with-Prometheus-4-Architecture-Overview.png)

![Image](https://grafana.com/docs/loki/latest/get-started/loki_architecture_components.svg)

![Image](https://grafana.com/media/docs/tempo/tempo_arch.png)

### Install via Helm

* Prometheus
* Grafana
* Loki
* Tempo

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm install monitoring grafana/kube-prometheus-stack
```

✔ SLIs / SLOs
✔ Alert fatigue lessons
✔ Incident simulations

---

## 7️⃣ Chaos & Failure Testing (Free)

### Tools

| Tool           | Purpose             |
| -------------- | ------------------- |
| kubectl delete | Manual chaos        |
| stress         | CPU/memory pressure |
| network-policy | Network failures    |

Examples:

```bash
kubectl delete pod app-pod
kubectl drain worker-node
```

✔ Real incident experience
✔ Postmortem practice

---

## 8️⃣ Terraform (Mostly Free)

### Where Terraform fits

* Local cluster objects
* Helm releases
* Simulated cloud infra

Use:

* Local backend
* Module structure
* Policy simulation

---

# 🟡 Optional Add-On 1: **Real Cloud (Free Tier Only)**

If you want **real cloud exposure** without risk:

### AWS Free Tier (optional)

* 1 VPC
* 1 EKS cluster (short-lived)
* Tear down same day

Use **budget alerts**.

---

# 🟡 Optional Add-On 2: OpenShift

![Image](https://www.redhat.com/rhdc/managed-files/styles/default_800/private/sysadmin/2023-04/openshift_local_console.png.webp?itok=Tfi-ub8r)

![Image](https://spiritedengineering.net/wp-content/uploads/2019/07/openshift.png)

![Image](https://access.redhat.com/webassets/avalon/d/OpenShift_Container_Platform-4.2-Operators-en-US/images/c8a00bac5df42a702ff1b826333610d0/olm-workflow.png)

### Use:

* **CodeReady Containers (CRC)**

```bash
crc setup
crc start
```

✔ Operators
✔ Routes
✔ SCC
✔ RBAC differences

---

# 📅 Day-0 Setup Checklist (Do This First)

### ✅ 1. Install tools

```bash
docker
kubectl
kind
helm
terraform
k9s
```

### ✅ 2. Create Kind cluster

```bash
kind create cluster --config kind.yaml
```

### ✅ 3. Setup GitHub repo

* `devops-lab`
* Enable Actions

### ✅ 4. Install Argo CD

* Connect repo
* Deploy sample app

### ✅ 5. Install monitoring

* Access Grafana
* Create first dashboard

---

# ⚠️ Common Mistake (Avoid This)

❌ Spinning too many tools
❌ Paying cloud bills
❌ Skipping documentation

✔ Build → Break → Fix → Document

---


