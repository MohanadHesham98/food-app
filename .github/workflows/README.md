# Food App CI/CD with GitHub Actions and Self-Hosted Runner

This guide explains how to set up GitHub Actions CI/CD for the Food App project using a self-hosted runner on your VM and Docker Hub for container images.

---

### Step 1: Create a Docker Hub Access Token

Important: Do not use your Docker password. Use an access token.

**Steps**:

- Log in to Docker Hub.

- Go to Account Settings → Security → New Access Token.

- Enter a name: github-actions.

- Set permissions to Read & Write.

- Copy the token; you'll use it as a GitHub secret.

---

### Step 2: Set Up a Self-Hosted Runner on Your VM

On GitHub, go to your repository → Settings → Actions → Runners → New self-hosted runner.

Select:

- OS: Linux

- Architecture: x64

GitHub will give you commands to download and configure the runner.

**On your VM**:
```
mkdir actions-runner && cd actions-runner
curl -o actions-runner-linux-x64.tar.gz -L https://github.com/actions/runner/releases/latest/download/actions-runner-linux-x64.tar.gz
tar xzf actions-runner-linux-x64.tar.gz
```

**Configure the runner**:
```
./config.sh \
  --url https://github.com/YourUsername/food-app \
  --token <YOUR_GITHUB_TOKEN>
```

- Runner name: food-vm-runner

- Labels: food

- Work folder: Press Enter for default

**Start the runner**:
```
./run.sh
```

**You should see**:
```
Listening for Jobs
```
This means the runner is ready to execute GitHub Actions jobs.

---

### Step 3: Add GitHub Secrets

**In your repository**:

**Settings → Secrets → Actions, add**:

Name	            Value
DOCKER_USERNAME	  your-docker-username
DOCKER_TOKEN	    the token you created

---

### Step 4: GitHub Actions Workflow

On GitHub, go to your repository → Actions → set up a workflow yourself

.github/workflows/deploy.yml

---

**deploy.yml**:
```
name: CI/CD Food App

on:
  push:
    branches: ["main"]

jobs:
  deploy:
    runs-on: self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Docker Login
        run: |
          echo "${{ secrets.DOCKER_TOKEN }}" | docker login \
          -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin

      - name: Build Docker Images
        run: |
          docker compose build

      - name: Push Docker Images
        run: |
          docker compose push

      - name: Deploy to Kubernetes
        run: |
          cd k8s
          kubectl apply -f namespace.yaml
          kubectl apply -f food-secrets.yaml
          kubectl apply -f mongodb/
          kubectl apply -f backend/
          kubectl apply -f frontend/
          kubectl apply -f admin/
```
---

### Step 5: Notes on Runner Paths

If your runner is running from /home/test/actions-runner, GitHub Actions clones the repository here:

/home/test/actions-runner/_work/food-app/food-app


The general path structure is:

<runner-path>/_work/<repo-name>/<repo-name>


Ensure ./run.sh is always active on the VM to listen for workflow jobs.
<img width="636" height="203" alt="image" src="https://github.com/user-attachments/assets/15e3fb78-41f3-43c8-a4f4-68e21208287a" />

