# CI/CD Pipeline Guide 🚀

## 📌 Overview

This repository contains everything you need to know about **CI/CD pipelines** for backend, mobile, and frontend applications. It covers setup, security, caching, rollback, scheduled deployments, and automation.

---

## 🔹 1. Setting Up a CI/CD Pipeline

### **📁 Folder Structure**

The workflow files are stored in:

```
.github/workflows/ci.yml
```

This path is **fixed** for GitHub Actions.

### **📜 Workflow Structure**

A typical workflow follows this structure:
1️⃣ **Trigger**: Defines when the pipeline runs (e.g., `push`, `pull_request`, `schedule`)
2️⃣ **Jobs**: Tasks that run on specified machines (e.g., `ubuntu-latest`, `macos-latest`)
3️⃣ **Steps**: Install dependencies, run tests, deploy, etc.

Example:

```yaml
on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

---

## 🔹 2. CI/CD Security Best Practices 🔐

✅ **Use GitHub Secrets** instead of hardcoding credentials

```yaml
env:
  AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
  AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

✅ **Restrict branch access** (e.g., only allow CI/CD on `main` branch)

```yaml
on:
  push:
    branches:
      - main
```

✅ **Require approvals for production deployments**

---

## 🔹 3. Rollback Strategy 🔄

Rollback ensures quick recovery if deployment fails.

### **✅ Rollback on Failure**

```yaml
run: aws s3 cp app.zip s3://my-app-bucket --profile aws-user || aws s3 cp backup.zip s3://my-app-bucket --profile aws-user
```

### **✅ Advanced Rollback: Versioning**

Use **blue-green deployment** or **canary releases** to gradually roll back to a stable version.

---

## 🔹 4. CI/CD Caching for Faster Builds ⚡

✅ **Cache Dependencies**

```yaml
- name: Cache dependencies
  uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

✅ **Cache Build Artifacts** (for reuse across jobs)

```yaml
- name: Cache build output
  uses: actions/cache@v3
  with:
    path: ./build
    key: build-${{ github.sha }}
```

---

## 🔹 5. Scheduled Deployments ⏳

✅ **Deploy at Midnight (UTC)**

```yaml
on:
  schedule:
    - cron: "0 0 * * *" # Runs daily at 12:00 AM UTC
```

✅ **Allow Manual Trigger**

```yaml
on:
  workflow_dispatch:
```

---

## 🔹 6. Automating CI/CD 🚀

✅ **Zero-Downtime Deployment** (Blue-Green, Canary)
✅ **Feature Flags for Safe Rollouts**
✅ **GitOps: Manage CI/CD with Git**

---

## 🎯 Next Steps

- **Secure CI/CD Further** (SAST, DAST, SBOM checks)
- **Dynamic Environments** (Temporary test environments for PRs)
- **Multi-cloud Deployments** (AWS, GCP, Azure)

**💡 Keep this README handy for future reference!** 🚀
