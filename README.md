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
