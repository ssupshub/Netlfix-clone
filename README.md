Here’s a `README.md` file tailored for a GitHub repository, incorporating all the necessary steps to set up the Netflix clone application using Kubernetes and Docker on AWS EC2 instances. This guide assumes that users will follow the instructions to deploy both Kubernetes and Docker setups, including obtaining an API key and running the application.

```markdown
# Netflix Clone Deployment Guide

This repository provides instructions for deploying a Netflix clone application using Kubernetes and Docker on AWS EC2 instances.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Kubernetes Deployment](#kubernetes-deployment)
  - [1. Launch EC2 Instances](#1-launch-ec2-instances)
  - [2. Connect to Instances](#2-connect-to-instances)
  - [3. Install Kubernetes](#3-install-kubernetes)
  - [4. Install Git](#4-install-git)
  - [5. Deploy the Application](#5-deploy-the-application)
- [Docker Deployment](#docker-deployment)
  - [1. Launch EC2 Instance](#1-launch-ec2-instance)
  - [2. Connect to Instance](#2-connect-to-instance)
  - [3. Install Docker](#3-install-docker)
  - [4. Obtain TMDB API Key](#4-obtain-tmdb-api-key)
  - [5. Set Up and Run the Application](#5-set-up-and-run-the-application)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- AWS EC2 account
- Basic knowledge of Kubernetes and Docker
- TMDB API key (for Docker setup)

## Kubernetes Deployment

### 1. Launch EC2 Instances

1. **Master Instance:**
   - **Type:** `t2.medium`
   - **OS:** Ubuntu
   - **Key:** PEM key for SSH
   - **Networking:** Allow all traffic and HTTP

2. **Cluster Instance:**
   - **OS:** Ubuntu
   - **Key:** PEM key for SSH
   - **Networking:** Default settings, adjust as needed

### 2. Connect to Instances

- **SSH into the Master Instance:**
  ```bash
  ssh -i "your-key.pem" ubuntu@<master-instance-ip>
  ```

- **SSH into the Cluster Instance:**
  ```bash
  ssh -i "your-key.pem" ubuntu@<cluster-instance-ip>
  ```

### 3. Install Kubernetes

On the Master Instance:
```bash
sudo apt update && sudo apt full-upgrade -y
git clone https://github.com/ssupshub/Kubernetes-file-.git
cd Kubernetes-file-
vim master.sh # Paste the content from the master file
chmod +x master.sh
./master.sh
chmod +x common.sh
./common.sh
```

- Connect the Cluster with the Master using `kubeadm` token:
  ```bash
  kubeadm token create --print-join-command
  ```

- Run the join command on the Cluster Instance.

- Verify the nodes:
  ```bash
  kubectl get nodes
  ```

### 4. Install Git

```bash
sudo apt install git
which git
```

### 5. Deploy the Application

- **Clone the Repository:**
  ```bash
  git clone https://github.com/Aj7Ay/Netflix-clone.git
  cd Netflix-clone/Kubernetes
  ```

- **Apply Deployment and Service:**
  ```bash
  kubectl apply -f deployment.yml
  kubectl apply -f service.yml
  ```

- **Check the Pods and Services:**
  ```bash
  kubectl get pods
  kubectl get services
  ```

- **Access the Application:**
  Visit [http://<your_instance_ip>:30007](http://<your_instance_ip>:30007) in your browser.

## Docker Deployment

### 1. Launch EC2 Instance

- **OS:** Ubuntu
- **Key:** PEM key for SSH
- **Networking:** Allow all traffic and HTTP

### 2. Connect to Instance

```bash
ssh -i "your-key.pem" ubuntu@<instance-ip>
```

### 3. Install Docker

```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install git
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

### 4. Obtain TMDB API Key

1. Create an account on [TMDB](https://www.themoviedb.org/).
2. Go to Settings > API and generate your API key.
3. Save the API key for use.

### 5. Set Up and Run the Application

- **Clone the Repository:**
  ```bash
  git clone https://github.com/Aj7Ay/Netflix-clone.git
  cd Netflix-clone
  ```

- **Build and Run the Docker Container:**
  ```bash
  docker build --build-arg TMDB_V3_API_KEY=your_api_key_here -t netflix-clone .
  docker run --name netflix-clone-website --rm -d -p 80:80 netflix-clone
  ```

  Replace `your_api_key_here` with your actual TMDB API key.

- **Verify Docker Containers:**
  ```bash
  docker images
  docker ps
  ```

- **Access the Application:**
  Visit [http://<your_instance_ip>:80](http://<your_instance_ip>:80) in your browser.

## Troubleshooting

- Ensure that your instances have the necessary security group rules to allow traffic on the required ports.
- Check logs for any errors in Kubernetes or Docker setups.
- For further assistance, refer to the [Kubernetes Documentation](https://kubernetes.io/docs/) and the [Docker Documentation](https://docs.docker.com/).

```

This `README.md` is structured to guide users through the process of deploying the application using both Kubernetes and Docker, and it includes essential details and troubleshooting tips. Adjust the placeholders (like `your-key.pem`, `your_instance_ip`, and `your_api_key_here`) with the appropriate values for your environment.
