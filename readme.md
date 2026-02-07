
# Terraform AWS VPC Project

This project demonstrates the provisioning of a custom AWS Virtual Private Cloud (VPC) using **Terraform**, following infrastructure-as-code best practices.

The environment was built and deployed entirely via the CLI and validated through real connectivity and application testing.

---

## Overview

The infrastructure includes:

- A custom VPC with CIDR block `10.0.0.0/16`
- Public and private subnets
- Internet Gateway for public internet access
- NAT Gateway for outbound internet access from private subnets
- Route tables with proper associations
- Security groups with controlled ingress/egress
- An EC2 instance running **Ubuntu 24.04**
- NGINX installed and serving a custom webpage

---

## Architecture

**High-level design:**

- Users access the application via the EC2 public IP
- Traffic flows through the Internet Gateway
- The EC2 instance resides in a public subnet
- Private subnet resources route outbound traffic via a NAT Gateway
- Network isolation and routing are explicitly defined using Terraform

## 🧩 Architecture Diagram

```text
┌──────────────────────────────────────────────┐
│                  Internet                    │
└───────────────────────┬──────────────────────┘
                        │
                ┌───────▼────────┐
                │ Internet Gateway│
                └───────┬────────┘
                        │
        ┌─────────────────────────────────────┐
        │              VPC                     │
        │           10.0.0.0/16                │
        │                                     │
        │   ┌───────────────┐   ┌──────────┐ │
        │   │ Public Subnet │   │ Private  │ │
        │   │ 10.0.1.0/24   │   │ Subnet   │ │
        │   │               │   │10.0.2.0/24│ │
        │   │  ┌─────────┐ │   │          │ │
        │   │  │  EC2    │ │   │          │ │
        │   │  │ NGINX   │ │   │          │ │
        │   │  │ Ubuntu  │ │   │          │ │
        │   │  └─────────┘ │   │          │ │
        │   │               │   │          │ │
        │   │ Route Table   │   │ Route    │ │
        │   │ 0.0.0.0/0 → IGW│  │ 0.0.0.0/0 → NAT│
        │   └───────────────┘   └──────┬───┘ │
        │                               │     │
        │                        ┌──────▼───┐ │
        │                        │ NAT GW   │ │
        │                        │ + EIP    │ │
        │                        └──────────┘ │
        │                                     │
        └─────────────────────────────────────┘

```
---

## Technology Stack

- **Terraform**
- **AWS VPC**
- **EC2**
- **NGINX**
- **Ubuntu 24.04 LTS**
- **Git & GitHub**
- **Linux CLI**

---

## Project Structure
```text
.
├── main.tf         # Core infrastructure resources
├── variables.tf   # Input variables
├── outputs.tf     # Output values
├── versions.tf    # Provider and Terraform version constraints
├── .gitignore
└── README.md

```
---

## Deployment Steps

Initialize Terraform:

terraform init


Review the execution plan:

terraform plan


Apply the configuration:

terraform apply


SSH into the EC2 instance using the generated key pair:

ssh -i <key.pem> ubuntu@<public-ip>

---

## Install and verify NGINX:

sudo apt update
sudo apt install nginx -y
curl localhost


## Access the web server via browser:

/http://18.170.213.129/

---

## Validation

Verified outbound internet access using curl google.com

Confirmed public IP via curl ifconfig.me

Confirmed NGINX service accessibility via browser

Confirmed SSH access via public IP

Confirmed correct routing and subnet placement in AWS Console

---

## Key Learnings

Practical VPC design and subnet segmentation

Terraform resource dependency management

Secure SSH access using key pairs

Understanding public vs private networking in AWS

Using Infrastructure as Code to build reproducible environments

Debugging connectivity, routing, and permissions in real systems

---

## Future Improvements

Add Application Load Balancer

Move EC2 to private subnet behind ALB

Add Auto Scaling Group

Introduce HTTPS with ACM

Modularise Terraform configuration

Add monitoring and logging

---

## Author

### Hassan Hussein
### People Ops × DevOps × Infrastructure
### Building real systems, learning in public.

