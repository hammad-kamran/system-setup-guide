# System Setup Guide

This repository provides a step-by-step guide for setting up Docker, Nginx, and a sample web application. Follow the instructions to configure your system as described.

## Tasks

1. Identify OS and system specifications.
2. Setup Docker and Nginx (Nginx as a service, not a container).
3. Setup a sample web application in Docker on a port other than 80.
4. Configure Nginx to route traffic to the sample web application.

## Prerequisites

- Administrative access to the server.
- Basic knowledge of Linux command-line operations.

## Steps

### 1. Identify OS and Specs

```bash
# Check OS
uname -a

# Check system specs
lscpu

### 2. Install Docker

Run the following script to install Docker:

```bash
./install_docker.sh

### 3. Build and Run the Sample Web Application
# Build the Docker image
sudo docker build -t sample-web-app .

# Run the Docker container on port 8080
sudo docker run -d -p 8080:80 sample-web-app

### 4. Setup Nginx
# Install Nginx
sudo apt-get install -y nginx

# Start Nginx service
sudo systemctl start nginx

# Enable Nginx to start on boot
sudo systemctl enable nginx

### 5. Configure Nginx
sudo nano /etc/nginx/sites-available/default

# Replace the content with the following configuration, adjusting your-domain.com with your actual domain:

server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

# Test and apply Nginx configuration changes:

sudo nginx -t
sudo systemctl reload nginx


