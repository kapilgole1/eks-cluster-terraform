# AWS VPC with Public and Private Subnets Terraform Module

This Terraform project provisions an **AWS Virtual Private Cloud (VPC)** with the following architecture:

- **VPC CIDR:** 10.0.0.0/16
- **Public Subnet:** 10.0.1.0/24
  - Has Internet Gateway (IGW) attached
  - Route table allows internet access (`0.0.0.0/0 → IGW`)
  - Hosts resources accessible from the internet
- **Private Subnet:** 10.0.2.0/24
  - No direct internet access (no NAT Gateway or IGW route)
  - Route table restricts outbound traffic only to public subnet
  - Can only communicate with resources in the public subnet
- **Security Groups:**
  - Public subnet SG allows inbound internet traffic (e.g., SSH) and traffic from private subnet
  - Private subnet SG allows inbound/outbound traffic only to/from public subnet

---

## Architecture Overview

```text
VPC (10.0.0.0/16)
├── Public Subnet (10.0.1.0/24)
│   ├── Internet Gateway (IGW)
│   ├── Route Table: 0.0.0.0/0 → IGW
│   └── Security Group: Allow inbound from internet and private subnet
│
└── Private Subnet (10.0.2.0/24)
    ├── Route Table: No internet route
    └── Security Group: Allow inbound/outbound only from/to public subnet
```

---

## Features

- VPC creation with specified CIDR block.
- Public and private subnet setup with proper route tables.
- Internet Gateway attached to the VPC and routed via public subnet.
- Security groups to restrict communication:
  - Public subnet SG allows inbound from internet and private subnet.
  - Private subnet SG allows inbound and outbound **only** from/to public subnet.
- No NAT Gateway is created, so private subnet cannot access the internet.

---

## Prerequisites

- Terraform v1.0 or newer installed ([Install Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli))
- AWS CLI installed and configured with credentials ([AWS CLI Setup](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html))
- AWS account with permissions to create networking resources

---

## Getting Started

1. **Clone the repository**

```bash
git clone https://github.com/your-username/terraform-aws-vpc-public-private.git
cd terraform-aws-vpc-public-private

