# Devops-Stage4-Infra

# Infrastructure Setup

## Overview
This project automates infrastructure provisioning and application deployment using **Terraform**, **Ansible**, **Docker**, and **Traefik**. The setup includes:

- Provisioning an **Azure Virtual Machine** with Terraform.
- Configuring the VM with necessary dependencies using Ansible.
- Deploying containerized services using Docker and managing traffic with Traefik.

## Technologies Used

- **Terraform**: Infrastructure as Code (IaC) for provisioning Azure resources.
- **Ansible**: Configuration management for setting up software on the VM.
- **Docker & Docker Compose**: Containerized application deployment.
- **Traefik**: Reverse proxy for routing traffic to services.

## Infrastructure Deployment

### 1. Terraform Configuration
Terraform provisions the following Azure resources:

- **Resource Group**: `devops-stage-rg`
- **Virtual Network**: `devops-vnet`
- **Subnet**: `devops-subnet`
- **Network Security Group (NSG)**: Allows SSH (22), HTTP (80), and HTTPS (443).
- **Public IP Address**: `devops-public-ip`
- **Network Interface (NIC)**: Associated with the NSG.
- **Linux Virtual Machine (VM)**: `devops-vm` running Ubuntu 22.04 LTS.

#### Terraform Deployment Steps
1. Initialize Terraform:
   ```sh
   terraform init
   ```
2. Plan the deployment:
   ```sh
   terraform plan
   ```
3. Apply the configuration:
   ```sh
   terraform apply -auto-approve
   ```
4. Retrieve the VM's public IP:
   ```sh
   terraform output public_ip
   ```

### 2. Ansible Configuration
Ansible is used to configure the VM by performing the following tasks:

- Install **Docker** and **Docker Compose**.
- Clone the application repository.
- Copy the `docker-compose.yml` file.
- Create a `letsencrypt` directory for SSL certificates.
- Start the application using Docker Compose.

#### Running Ansible
The Terraform script automatically transfers and executes the Ansible playbook on the VM. However, you can also run it manually:

```sh
ansible-playbook -i inventory ansible-playbook.yml
```

### 3. Docker & Traefik Configuration
The project uses **Docker Compose** to manage services, while **Traefik** acts as the reverse proxy to handle incoming requests securely.

#### Services Deployed:

- **Traefik**: Reverse proxy and load balancer.
- **Redis**: Message queue service.
- **Users API**: Handles user-related operations.
- **Auth API**: Handles authentication services.

#### Deploying the Application
If the Ansible setup completes successfully, the application should be running. You can verify and restart services manually:

```sh
docker-compose up -d
```

## Accessing the Services

- **APIs**:
  - `https://users.starrinthecloud.name.ng/api/users`
  - `https://auth.starrinthecloud.name.ng/api/auth`
- **Traefik Dashboard**:
  - `http://<public-ip>:8080`

## Cleanup
To tear down the infrastructure, run:

```sh
terraform destroy -auto-approve
```

This setup ensures a fully automated and reproducible infrastructure for seamless deployment and scaling.
