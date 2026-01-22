# 🚀 Deployment Guide – DevOps Cloud Automation Platform

This project demonstrates an end-to-end DevOps workflow including local development, Docker containerization, AWS EC2 deployment, Nginx reverse proxy configuration, GitHub SSH authentication, and CI/CD readiness with Jenkins.

---

### Local Development Setup (Windows)

Set up the project locally for development and testing.

---

### Prerequisites

Ensure the following tools are installed on your system:

- Python 3.13+
- Git
- Docker Desktop (running)

---

### Verify Installation

Verify that all required tools are installed correctly.

```bash
python --version
git --version
docker --version
```
### Git Configuration
Configure Git with your global username and email.

```bash
git config --global user.name "Karan"
git config --global user.email "kr74747@gmail.com"
git config --global --list
```
### Python Virtual Environment Setup
Create and activate a Python virtual environment to isolate project dependencies and maintain a clean development setup.

```bash
python -m venv venv
venv\Scripts\activate
python -m pip install --upgrade pip
```
### Install Application Dependencies
Install all required Python dependencies for the project.

```bash
pip install -r requirements.txt
```
### Run Application Locally
Run the Flask application locally.

```bash
python app.py
```
The application will be available at:

```bash
http://127.0.0.1:5000
```
### Docker Containerization (Local)
Containerize the application using Docker.

### Build Docker Image
Build the Docker image for the Flask application.

```bash
docker build -t devops-cloud-app .
```
### Run Docker Container
Run the Docker container and expose port 5000.

```bash
docker run -d -p 5000:5000 --name devops-container devops-cloud-app
```
### Verify Running Containers
Verify that the Docker container is running.

```bash
docker ps
```
### Stop and Remove Docker Container
Stop and remove the running container when required.
```bash
docker stop devops-container
docker rm devops-container
```
### Version Control with GitHub
Track and push project changes to GitHub.
Add, Commit, and Push Code
Add project files, commit changes, and push to GitHub.

```bash
git add app.py requirements.txt Dockerfile Jenkinsfile
git commit -m "Dockerized Flask Application and CI/CD pipeline"
git push
```
### AWS EC2 Deployment
Deploy the Dockerized application on AWS EC2.

### Connect to EC2 Instance
Set correct permissions for the key and connect to the EC2 instance.

```bash
chmod 400 devops-key.pem
ssh -i devops-key.pem ubuntu@<EC2_PUBLIC_IP>
```
### Update System Packages
Update the package list on the EC2 instance.

```bash
sudo apt update
```
### Install Docker and Git on EC2
Install Docker and Git on the EC2 instance.

```bash
sudo apt install docker.io git -y
```
### Enable Docker Service
Start Docker, enable it at boot, and add the user to the Docker group.

```bash
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
exit
```
 Re-login to apply Docker group permissions.

### Clone Repository on EC2
Clone the GitHub repository on the EC2 instance.

```bash
git clone https://github.com/techSavvy1001/devops-cloud-automation.git
cd devops-cloud-automation
```
### Build and Run Docker Container on EC2
Build the Docker image and run the container on EC2.

```bash
docker build -t devops-cloud-app .
docker run -d -p 5000:5000 --name devops-container devops-cloud-app
docker ps
```
### Nginx Reverse Proxy Setup
Configure Nginx as a reverse proxy to expose the application on port 80.

### Install Nginx
Install Nginx on the EC2 instance.

```bash
sudo apt install nginx -y
```
### Start and Enable Nginx
Start Nginx and enable it on system boot.

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```
### Configure Nginx Server Block
Create a new Nginx configuration file.

```bash
sudo nano /etc/nginx/sites-available/devops-cloud
```

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
### Enable Nginx Configuration
Enable the Nginx configuration and reload the service.

```bash
sudo ln -s /etc/nginx/sites-available/devops-cloud /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```
The application is now accessible at:
```
http://<EC2_PUBLIC_IP>
```
### GitHub SSH Authentication (EC2)
Secure Git operations on EC2 using SSH authentication.

### Generate SSH Key
Generate an SSH key on the EC2 instance.

```bash
ssh-keygen -t ed25519 -C "kr74747@gmail.com"
```
### Copy Public SSH Key
Copy the public key to add it to GitHub.

```bash
cat ~/.ssh/id_ed25519.pub
```
Add this key to GitHub → Settings → SSH & GPG Keys.

### Switch Repository to SSH
Update the Git remote URL to use SSH.

```bash
git remote set-url origin git@github.com:techSavvy1001/devops-cloud-automation.git
git remote -v
git push
```
### CI/CD Readiness (Jenkins)
Prepare the project for CI/CD using Jenkins.

### Jenkins Pipeline Configuration
Commit and push Jenkins pipeline configuration.

```bash
git add Jenkinsfile
git commit -m "Enhanced Jenkins pipeline for Docker-based Deployment"
git push
```
## Final Architecture
```scss
Client
  ↓
Nginx (Port 80)
  ↓
Docker Container (Flask App on Port 5000)
  ↓
AWS EC2
```

## 🔐 Security Notes

```
- SSH private keys (`.pem`, `id_ed25519`) are never committed to the repository.
- EC2 public IPs are represented using placeholders.
- GitHub authentication is handled via SSH keys.
- No secrets or credentials are stored in the codebase.
```
