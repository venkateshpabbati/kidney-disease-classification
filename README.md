# Kidney Disease Classification

## Overview
The **Kidney Disease Classification** project is a machine learning-based solution designed to classify kidney disease using medical data. The project leverages **MLflow** and **DVC** for experiment tracking and version control and supports deployment using **AWS CICD with GitHub Actions**.

---

## Project Workflow
To ensure smooth development and deployment, follow the steps below:

1. **Update Configuration Files**
   - Modify `config.yaml` for model training configurations.
   - (Optional) Update `secrets.yaml` for credentials and sensitive information.
   - Adjust `params.yaml` to tune hyperparameters.

2. **Modify Entity & Configuration Manager**
   - Update `config_entity.py` inside the `entity` folder.
   - Modify configuration management scripts inside `src/cnnClassifier/config`.

3. **Update Components & Pipelines**
   - Modify relevant scripts inside `src/cnnClassifier/components`.
   - Update pipeline scripts in `src/cnnClassifier/pipeline`.

4. **Update Main Execution File**
   - Modify `main.py` to execute the end-to-end pipeline.

5. **Modify DVC and Deployment Scripts**
   - Update `dvc.yaml` to manage dependencies and pipeline stages.
   - Ensure `app.py` is updated for serving the model via Flask API.

---

## Setup & Execution

### Step 1: Clone the Repository
```bash
git clone https://github.com/venkateshpabbati/Kidney-Disease-Classification.git
cd Kidney-Disease-Classification
```

### Step 2: Create a Virtual Environment
```bash
python -m venv .venv
```

#### Windows:
```bash
.venv\Scripts\Activate.ps1
```

#### macOS/Linux:
```bash
source .venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Run the Application
```bash
python app.py
```

### Step 5: Access the Application
Open your browser and go to `http://localhost:<PORT>` (replace `<PORT>` with the actual port number).

---

## Experiment Tracking with MLflow

### MLflow Documentation:
- [Official Documentation](https://mlflow.org/docs/latest/index.html)
- [MLflow Tutorial](https://youtu.be/qdcHHrsXA48?si=bD5vDS60akNphkem)

### Run MLflow UI
```bash
mlflow ui
```

### Using DagsHub for Remote Tracking
```bash
export MLFLOW_TRACKING_URI=https://dagshub.com/venkateshpabbati/Kidney-Disease-Classification.mlflow
export MLFLOW_TRACKING_USERNAME=username 
export MLFLOW_TRACKING_PASSWORD=password
```

### Using Mlflow tracking
```bash
import dagshub
dagshub.init(repo_owner='venkateshpabbati', repo_name='Kidney-Disease-Classification', mlflow=True)

import mlflow
with mlflow.start_run():
  mlflow.log_param('parameter name', 'value')
  mlflow.log_metric('metric name', 1)
```
---

## Version Control with DVC

### Initialize DVC
```bash
dvc init
```

### Reproduce the Pipeline
```bash
dvc repro
```

### Visualize Pipeline DAG
```bash
dvc dag
```

---

## AWS CI/CD Deployment with GitHub Actions

### Step 1: AWS Account Setup
- Log in to AWS Console.
- Create an IAM user with the following access:
  - **AmazonEC2ContainerRegistryFullAccess**
  - **AmazonEC2FullAccess**

### Step 2: Create an ECR Repository
- Create a repository in **Amazon ECR** to store the Docker image.
- Note the **ECR URI** (e.g., `566373416292.dkr.ecr.us-east-1.amazonaws.com/kidney-disease-classifier`).

### Step 3: Launch an EC2 Instance (Ubuntu)

### Step 4: Install Docker on EC2
```bash
sudo apt-get update -y
sudo apt-get upgrade -y
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

### Step 5: Configure EC2 as a Self-Hosted GitHub Runner
Go to GitHub repository:
- Navigate to **Settings > Actions > Runners**
- Click **New Self-hosted Runner** and follow the setup instructions.

### Step 6: Set GitHub Secrets
Configure GitHub Secrets under `Settings > Secrets and Variables > Actions`:
```bash
AWS_ACCESS_KEY_ID=<YOUR_AWS_ACCESS_KEY_ID>
AWS_SECRET_ACCESS_KEY=<YOUR_AWS_SECRET_ACCESS_KEY>
AWS_REGION=us-east-1
AWS_ECR_LOGIN_URI=566373416292.dkr.ecr.us-east-1.amazonaws.com
ECR_REPOSITORY_NAME=kidney-disease-classifier
```

### Step 7: Deployment Process
1. **Build Docker Image**
```bash
docker build -t kidney-disease-classifier .
```

2. **Authenticate Docker with ECR**
```bash
eval $(aws ecr get-login --no-include-email --region us-east-1)
```

3. **Push Image to ECR**
```bash
docker tag kidney-disease-classifier:latest 566373416292.dkr.ecr.us-east-1.amazonaws.com/kidney-disease-classifier:latest
docker push 566373416292.dkr.ecr.us-east-1.amazonaws.com/kidney-disease-classifier:latest
```

4. **Run Docker Container on EC2**
```bash
docker run -d -p 5000:5000 566373416292.dkr.ecr.us-east-1.amazonaws.com/kidney-disease-classifier:latest
```

### Step 8: Access the Deployed Model
Navigate to `http://<EC2_PUBLIC_IP>:5000`

---

## About MLflow & DVC
### MLflow:
- Provides a **production-grade** solution for experiment tracking.
- Enables **model logging and versioning**.
- Allows easy **model deployment and reproducibility**.

### DVC:
- A **lightweight** tool for **proof-of-concept (POC) projects**.
- Manages **experiment tracking and dataset versioning**.
- Supports **pipeline orchestration**.

---

## Conclusion
This project provides a robust and scalable workflow for **kidney disease classification**, leveraging **MLflow for experiment tracking, DVC for version control, and AWS CI/CD for automated deployment**. By following this guide, you can effectively train, track, and deploy your models in a production-ready environment.
