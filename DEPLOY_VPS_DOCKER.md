# VPS Deployment Guide for Ubuntu 22.04

## 1. Introduction
This guide provides step-by-step instructions for deploying applications using Docker on an Ubuntu 22.04 VPS.

## 2. Prerequisites
- A VPS running Ubuntu 22.04
- Root or sudo access to the server

## 3. Install Docker
To install Docker, run the following commands:

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

## 4. Install Docker Compose
To install Docker Compose, execute:

```bash
sudo apt install -y docker-compose
```

## 5. Running Your First Container
You can run a test container to verify the installation:

```bash
sudo docker run hello-world
```

## 6. Docker Compose Execution
To use Docker Compose, create a `docker-compose.yml` file and define your services. Here's a simple example:

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
```

Run your application:

```bash
sudo docker-compose up -d
```

## 7. SSL Setup with Let's Encrypt
To secure your application with SSL, follow these steps:

1. Install Certbot:
   ```bash
   sudo apt install certbot python3-certbot-nginx
   ```

2. Obtain an SSL certificate:
   ```bash
   sudo certbot --nginx
   ```

3. Set up automatic renewal:
   ```bash
   sudo certbot renew --dry-run
   ```

## 8. Monitoring
You can monitor Docker containers using a variety of tools. A popular choice is `Portainer`, which provides a web interface:

```bash
sudo docker run -d -p 9000:9000 --name portainer --restart always \
   -v /var/run/docker.sock:/var/run/docker.sock \
   portainer/portainer
```

## 9. Conclusion
Your VPS is now set up with Docker, ready to deploy applications securely and efficiently.