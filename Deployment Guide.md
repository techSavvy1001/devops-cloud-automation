🚀 Deployment Guide (AWS EC2 + Docker + Nginx)

This project is deployed on AWS EC2 using Docker for containerization and Nginx as a reverse proxy.
The setup follows production-style DevOps best practices.

🔧 Prerequisites

AWS EC2 instance (Ubuntu 22.04)

SSH access using .pem key

Open ports in Security Group:

22 (SSH)

80 (HTTP)

5000 (Application – internal)

🖥️ Server Setup

SSH into the EC2 instance:

ssh -i devops-key.pem ubuntu@<EC2_PUBLIC_IP>


Update the system and install required tools:

sudo apt update
sudo apt install docker.io git -y


Enable Docker:

sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
exit


Re-login to apply Docker permissions.

📦 Application Deployment (Docker)

Clone the repository:

git clone https://github.com/techSavvy1001/devops-cloud-automation.git
cd devops-cloud-automation


Build the Docker image:

docker build -t devops-cloud-app .


Run the container:

docker run -d -p 5000:5000 --name devops-container devops-cloud-app


Verify:

docker ps


The application now runs internally on port 5000.

🌐 Nginx Reverse Proxy Setup

Install Nginx:

sudo apt install nginx -y


Start and enable Nginx:

sudo systemctl start nginx
sudo systemctl enable nginx


Create Nginx configuration:

sudo nano /etc/nginx/sites-available/devops-cloud


Add:

server {
    listen 80;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}


Enable configuration:

sudo ln -s /etc/nginx/sites-available/devops-cloud /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx

🔐 GitHub SSH Authentication (EC2)

Generate SSH key:

ssh-keygen -t ed25519 -C "kr74747@gmail.com"


Copy public key:

cat ~/.ssh/id_ed25519.pub


Add the key to GitHub → Settings → SSH and GPG keys.

Switch repository to SSH:

git remote set-url origin git@github.com:techSavvy1001/devops-cloud-automation.git
git push

✅ Final Result

Application accessible via EC2 public IP (port 80)

Dockerized backend running on port 5000

Nginx handles HTTP traffic and routing

Secure GitHub access via SSH

CI/CD-ready repository with Jenkinsfile
