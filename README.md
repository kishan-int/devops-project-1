# DevOps Project 1 — Automated CI/CD Pipeline with AWS

## Overview
A production-style CI/CD pipeline that automatically builds, pushes, and deploys a containerized Python Flask application to AWS EC2 whenever code is pushed to the main branch.

## Architecture
Developer pushes code to GitHub
↓
GitHub Actions triggers pipeline
↓
Docker image built from Dockerfile
↓
Image pushed to AWS ECR (private registry)
↓
GitHub Actions SSHs into AWS EC2
↓
Latest image pulled and redeployed
↓
App live on http://EC2-PUBLIC-IP:5000


## Tech Stack
| Tool | Purpose |
|---|---|
| Python Flask | Web application |
| Docker | Containerization |
| GitHub Actions | CI/CD pipeline |
| AWS ECR | Container image registry |
| AWS EC2 | Cloud server / deployment target |
| IAM Roles | Secure AWS authentication |

## Pipeline Flow
1. Code pushed to `main` branch
2. GitHub Actions spins up Ubuntu runner
3. Authenticates with AWS using IAM credentials stored as GitHub Secrets
4. Builds Docker image and tags with git commit SHA + `latest`
5. Pushes both tags to AWS ECR
6. SSHs into EC2 instance
7. Pulls latest image from ECR
8. Stops old container, starts new one on port 5000

## Key Concepts Demonstrated
- Container-based deployments
- Secrets management via GitHub Secrets
- IAM roles for EC2 to ECR access
- Zero-downtime redeployment pattern
- Infrastructure security (least privilege IAM)

## Project Structure

devops-project-1/
├── .github/
│ └── workflows/
│ └── deploy.yml # GitHub Actions pipeline
├── k8s/
│ ├── deployment.yaml # Kubernetes deployment manifest
│ └── service.yaml # Kubernetes service manifest
├── app.py # Flask application
├── requirements.txt # Python dependencies
└── Dockerfile # Container build instructions


## How to Run Locally
```bash
# Build image
docker build -t flask-app .

# Run container
docker run -p 5000:5000 flask-app

# Visit
http://localhost:5000
```

## Security Notes
- AWS credentials never stored in code — managed via GitHub Secrets
- EC2 uses IAM role for ECR access (no hardcoded keys on server)
- SSH key stored securely as GitHub Secret