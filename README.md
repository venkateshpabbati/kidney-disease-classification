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
export MLFLOW_TRACKING_USERNAME=entbappy
export MLFLOW_TRACKING_PASSWORD=6824692c47a369aa6f9eac5b10041d5c8edbcef0
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





# Kidney-Disease-Classification

## Workflows

1. Update config.yaml
2. Update secrets.yaml [Optional]
3. Update params.yaml
4. Update the entity
5. Update the configuration manager in src config
6. Update the components
7. Update the pipeline 
8. Update the main.py
9. Update the dvc.yaml
10. app.py

# How to run?
### STEPS:

Clone the repository

```bash
git clone https://github.com/venkateshpabbati/Kidney-Disease-Classification
```
### STEP 01- Create a virtual environment after opening the repository

```bash
pythonn -m venv .venv
```

```bash
.venv\Scripts\Activate.ps1
```


### STEP 02- install the requirements
```bash
pip install -r requirements.txt
```

```bash
# Finally run the following command
python app.py
```

Now,
```bash
open up you local host and port
```






## MLflow

- [Documentation](https://mlflow.org/docs/latest/index.html)

- [MLflow tutorial](https://youtu.be/qdcHHrsXA48?si=bD5vDS60akNphkem)

##### cmd
- mlflow ui

### dagshub
[dagshub](https://dagshub.com/)

MLFLOW_TRACKING_URI=https://dagshub.com/entbappy/Kidney-Disease-Classification-MLflow-DVC.mlflow \
MLFLOW_TRACKING_USERNAME=entbappy \
MLFLOW_TRACKING_PASSWORD=6824692c47a369aa6f9eac5b10041d5c8edbcef0 \
python script.py

Run this to export as env variables:

```bash

export MLFLOW_TRACKING_URI=https://dagshub.com/entbappy/Kidney-Disease-Classification-MLflow-DVC.mlflow

export MLFLOW_TRACKING_USERNAME=entbappy 

export MLFLOW_TRACKING_PASSWORD=6824692c47a369aa6f9eac5b10041d5c8edbcef0

```


### DVC cmd

1. dvc init
2. dvc repro
3. dvc dag


## About MLflow & DVC

MLflow

 - Its Production Grade
 - Trace all of your expriements
 - Logging & taging your model


DVC 

 - Its very lite weight for POC only
 - lite weight expriements tracker
 - It can perform Orchestration (Creating Pipelines)



# AWS-CICD-Deployment-with-Github-Actions

## 1. Login to AWS console.

## 2. Create IAM user for deployment

	#with specific access

	1. EC2 access : It is virtual machine

	2. ECR: Elastic Container registry to save your docker image in aws


	#Description: About the deployment

	1. Build docker image of the source code

	2. Push your docker image to ECR

	3. Launch Your EC2 

	4. Pull Your image from ECR in EC2

	5. Lauch your docker image in EC2

	#Policy:

	1. AmazonEC2ContainerRegistryFullAccess

	2. AmazonEC2FullAccess

	
## 3. Create ECR repo to store/save docker image
    - Save the URI: 566373416292.dkr.ecr.us-east-1.amazonaws.com/chicken

	
## 4. Create EC2 machine (Ubuntu) 

## 5. Open EC2 and Install docker in EC2 Machine:
	
	
	#optinal

	sudo apt-get update -y

	sudo apt-get upgrade
	
	#required

	curl -fsSL https://get.docker.com -o get-docker.sh

	sudo sh get-docker.sh

	sudo usermod -aG docker ubuntu

	newgrp docker
	
# 6. Configure EC2 as self-hosted runner:
    setting>actions>runner>new self hosted runner> choose os> then run command one by one


# 7. Setup github secrets:

    AWS_ACCESS_KEY_ID=

    AWS_SECRET_ACCESS_KEY=

    AWS_REGION = us-east-1

    AWS_ECR_LOGIN_URI = demo>>  566373416292.dkr.ecr.ap-south-1.amazonaws.com

    ECR_REPOSITORY_NAME = simple-app
