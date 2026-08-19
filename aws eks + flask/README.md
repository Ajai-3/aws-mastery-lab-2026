# EKS + Flask + GitHub Actions — Full CI/CD Pipeline Master Guide

Welcome to the **EKS + Flask + GitHub Actions Master Guide**. This document breaks down an end-to-end containerized application CI/CD deployment on Amazon EKS, covering infrastructure provisioning, containerization, Kubernetes orchestration, pipeline security, and scenario-based interview questions.

---

## Covered Topics

### 1. [01 - Overview & Concept](#1-overview--concept)
- What this pipeline actually is (Terraform, Flask, Kubernetes, GitHub Actions).
- Real-world scenario & GitOps-lite concept vs ArgoCD/Flux.

### 2. [02 - Architecture & CI/CD Flow](#2-architecture--cicd-flow)
- End-to-end deployment workflow from `git push` to EKS pod rollout.
- Visual flow diagram of the build, push, authenticate, and deploy steps.

### 3. [03 - Code & Repository Structure](#3-code--repository-structure)
- Standard repository layout for application, container, Kubernetes, Terraform, and CI/CD workflow files.

### 4. [04 - Component Breakdown & Resource Mapping](#4-component-breakdown--resource-mapping)
- Roles of each project file (`main.tf`, `app.py`, `Dockerfile`, `deployment.yaml`, `deploy.yml`).
- EC2 Worker Nodes (`desired_size`) vs Kubernetes App Pods (`replicas`).

### 5. [05 - Critical Production Pitfalls & Fixes](#5-critical-production-pitfalls--fixes)
- **Problem 1:** Static AWS credentials in GitHub Secrets vs OIDC Federation.
- **Problem 2:** Fragile `sed` tag replacement in YAML vs Kustomize / Helm.

### 6. [06 - DevOps Scenario Interview Q&A](#6-devops-scenario-interview-qa)
- **Q1:** Pipeline build succeeds but app doesn't update in production.
- **Q2:** Employee offboarding security risk with static credentials.
- **Q3:** Achieving zero-downtime deploys with Readiness / Liveness probes.
- **Q4:** Pods stuck in `Pending` due to Terraform `max_size` caps.
- **Q5:** Kubernetes Service `type: LoadBalancer` vs `ClusterIP` (and NLB upgrade).

### 7. [07 - Study & Revision Guide](#7-study--revision-guide)
- Practice checklist for mastering CI/CD security and container orchestration.

---

## Quick Navigation

| Concept & Architecture | Structure & Components | Pitfalls & Fixes | Scenario Q&A & Guide |
| :---: | :---: | :---: | :---: |
| [01 Overview](#1-overview--concept) \| [02 Flow](#2-architecture--cicd-flow) | [03 Structure](#3-code--repository-structure) \| [04 Components](#4-component-breakdown--resource-mapping) | [05 Pitfalls](#5-critical-production-pitfalls--fixes) | [06 Scenario Q&A](#6-devops-scenario-interview-qa) \| [07 Guide](#7-study--revision-guide) |

---

## 1. Overview & Concept

### What is this, in plain terms

This is a **complete deployment pipeline**. Four separate components working together:

1. **Terraform** — builds the infrastructure (VPC, EKS cluster) — the "house"
2. **Flask app + Dockerfile** — your actual application, packaged into a container — the "furniture"
3. **Kubernetes manifests** — tells EKS how to run that container (how many copies, what port) — the "furniture placement instructions"
4. **GitHub Actions** — automates the whole flow: code push → build → deploy, with zero manual steps

**One-line summary:** you push code to GitHub → GitHub Actions builds a Docker image → pushes it to ECR (AWS's container registry) → tells the EKS cluster to run the new image → old pods get replaced with new ones.

### Real-world scenario — where this is actually used

This exact pattern is used at almost every startup with a containerized backend. Example:

- A company has a Flask/FastAPI backend API
- Every time a developer merges code to `main`, they want it live in production automatically — no one manually SSHing into servers or running `kubectl apply` by hand
- This pipeline does exactly that: **push code → it's live**, typically in 2-5 minutes

You'll see this pattern called **GitOps-lite** — not full GitOps (that would use ArgoCD/Flux watching the repo), but "CI does the deploy directly." Good enough for small-to-mid teams. Larger teams usually move to ArgoCD later — worth knowing that distinction for an interview.

---

## 2. Architecture & CI/CD Flow

```
Developer pushes to main
        │
        ▼
GitHub Actions triggers
        │
        ▼
Build Docker image (from Dockerfile)
        │
        ▼
Push image to ECR (tagged with git commit SHA)
        │
        ▼
Update kubeconfig (authenticate to EKS cluster)
        │
        ▼
kubectl apply → EKS pulls new image, replaces old pods
        │
        ▼
2 Flask pods running, behind a LoadBalancer Service
```

---

## 3. Code & Repository Structure

```
.
├── app.py                          # Flask application
├── requirements.txt                # Python dependencies
├── Dockerfile                      # Container build instructions
├── k8s/
│   └── deployment.yaml             # Kubernetes Deployment + Service
├── terraform/
│   ├── main.tf                     # VPC + EKS cluster provisioning
│   └── outputs.tf                  # Cluster endpoint/name outputs
└── .github/
    └── workflows/
        └── deploy.yml              # CI/CD pipeline definition
```

---

## 4. Component Breakdown & Resource Mapping

### What each piece is actually doing (quick reference)

| File | Job |
|---|---|
| `main.tf` | Creates the VPC (network) and EKS cluster (compute) — the infrastructure |
| `app.py` | The actual Flask API code |
| `Dockerfile` | Packages the Flask app + its dependencies into a portable container image |
| `deployment.yaml` | Tells EKS: run 2 copies of this container, expose it via a load balancer |
| `deploy.yml` | Automates: build image → push to ECR → deploy to EKS, on every push to `main` |

**Why `desired_size = 2` in Terraform but `replicas: 2` in Kubernetes YAML — two different things:**
- Terraform's `desired_size` = how many **EC2 worker nodes** (servers) exist in the cluster
- Kubernetes `replicas` = how many **pods** (app instances) are running
- One node can run multiple pods — they're not the same number, don't confuse them in an interview

---

## 5. Critical Production Pitfalls & Fixes

### Problem 1: Static AWS access keys in GitHub Secrets

The workflow uses:
```yaml
aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

This is a **long-lived static credential** sitting in GitHub. If GitHub Secrets ever leak (misconfigured workflow, malicious PR from a fork, compromised repo), someone has standing AWS access until you manually rotate it.

**Better practice:** use **OIDC federation** — GitHub Actions assumes a temporary IAM role with no stored keys at all. This is the same principle as IRSA you learned earlier — temporary, scoped, auto-expiring credentials instead of a static key sitting around.

```yaml
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789012:role/github-actions-deploy-role
    aws-region: us-east-1
```
No secrets needed — GitHub and AWS trust each other directly via OIDC.

### Problem 2: `sed` to inject image tags into YAML is fragile

```yaml
sed -i "s|\${{ ECR_REGISTRY }}|$ECR_REGISTRY|g" k8s/deployment.yaml
```

This works, but it's a hack — string-replacing placeholder text in a YAML file. It breaks easily (typo in the placeholder, YAML formatting changes) and isn't how production teams do it.

**Better practice:** use `kustomize` (built into `kubectl`) or `envsubst` for proper templating, or a tool like Helm for anything beyond a single-container app.

---

## 6. DevOps Scenario Interview Q&A

### Q1: A developer pushes to `main`. The GitHub Actions build succeeds, but the app never updates in production. What do you check?

**Answer**
- Check `kubectl rollout status deployment/flask-app` — did the rollout actually complete, or is it stuck?
- Check if the new pods are in `CrashLoopBackOff` — image built fine, but app crashes on start
- Check if `sed` actually replaced the placeholders correctly — if the sed pattern didn't match, the old image tag is still in the YAML, so nothing changed
- Check ECR — did the image actually get pushed with the right tag?

---

### Q2: Your GitHub Actions workflow has access keys stored as secrets. A former employee's access was supposed to be revoked, but deploys still succeed. Why, and what's the fix?

**Answer**
- The IAM access keys in GitHub Secrets are independent of any individual's personal AWS access — revoking a person's account doesn't touch the shared static keys
- This is exactly the static-credential risk — the keys don't belong to a person, so "offboarding" them is easy to forget
- Fix: move to OIDC federation (temporary, role-based, no stored keys) so there's nothing static to leak or forget to revoke

---

### Q3: You need zero-downtime deployments — users should never see an error during a deploy. Does this current setup guarantee that?

**Answer**
- Not by default. `kubectl apply` triggers a **rolling update**, but if there's no **readiness probe** defined in `deployment.yaml`, Kubernetes might send traffic to a new pod before it's actually ready to handle requests
- Missing from this manifest: `readinessProbe` and `livenessProbe` — this file doesn't have them
- Fix: add both probes so Kubernetes only routes traffic to pods that are confirmed healthy, and restarts pods that hang

---

### Q4: Terraform's `eks_managed_node_groups` has `max_size = 3`. Traffic spikes and you need more capacity than 3 nodes can handle. What happens, and what's the fix?

**Answer**
- Nothing happens automatically — Terraform's `max_size` is a hard ceiling. Kubernetes Cluster Autoscaler (if enabled) can't add a 4th node because Terraform capped it at 3
- Pods will stay stuck in `Pending` state — not enough node capacity to schedule them
- Fix: this needs both a Terraform change (raise `max_size`) AND Cluster Autoscaler configured to actually use that headroom — the two need to work together, one isn't enough

---

### Q5: Why does the Service use `type: LoadBalancer` instead of `type: ClusterIP`?

**Answer**
- `ClusterIP` only makes the app reachable inside the cluster — no external access
- `LoadBalancer` type on EKS automatically provisions an AWS load balancer (Classic ELB by default) that routes external internet traffic into the cluster
- This is how outside users actually reach the Flask app — without it, the app would only be reachable from other pods inside the same cluster

**Follow-up to expect:** "Classic ELB is outdated — what would you use instead?" Answer: add an annotation to provision a Network Load Balancer (NLB) instead, which is the modern recommended choice on EKS.

---

## 7. Study & Revision Guide

- Re-read the two flagged problems (static keys, sed hack) until you can explain WHY they're problems, not just that they exist
- Walk through the architecture flow diagram out loud, service by service, without looking
- Answer the 5 scenario questions from memory, then compare against the answers here