# Infrastructure Setup Guide

## Secure Medical Data Processing Infrastructure Using VPC Endpoints

This document provides the step-by-step implementation guide used to deploy a **secure medical data processing environment on AWS**.

The goal of this architecture is to ensure that backend compute resources can securely access medical datasets stored in **Amazon S3** without exposing traffic to the public internet.

The infrastructure simulates a healthcare organization deploying a **private medical data processing server** that retrieves data from a secure storage layer.

The architecture includes:

* Custom Virtual Private Cloud (VPC)
* Public subnet for administrative access
* Private subnet for sensitive workloads
* Bastion Host for secure administration
* Private EC2 processing instance
* VPC Gateway Endpoint for private S3 access

All data transfers between compute and storage occur **within the AWS network**.

---

# Step 1 — Access AWS Environment

Sign in to the AWS Management Console using your organization's cloud account credentials.

Ensure that the working region is set to:

US East (N. Virginia) — `us-east-1`

This region is commonly used for development environments and lab simulations.

---

# Step 2 — Create the Virtual Private Cloud

Create a dedicated network environment for the medical data processing system.

Navigate to:

Services → VPC → Your VPCs → Create VPC

Configuration:

Name Tag
`Medical-Data-VPC`

IPv4 CIDR Block
`192.168.0.0/26`

IPv6 CIDR Block
None

Tenancy
Default

Create the VPC.

This VPC will host all resources for the secure medical processing environment.

---

# Step 3 — Create and Attach an Internet Gateway

The internet gateway allows controlled external access for administrative systems such as the Bastion Host.

Navigate to:

VPC → Internet Gateways → Create Internet Gateway

Configuration:

Name Tag
`Medical-VPC-InternetGateway`

After creation:

Actions → Attach to VPC

Select:

`Medical-Data-VPC`

This gateway will allow controlled internet access to the **public subnet only**.

---

# Step 4 — Create Public and Private Subnets

The network will be segmented into two subnets:

Public subnet
Hosts the Bastion Host used for administration.

Private subnet
Hosts backend medical processing servers.

Navigate to:

VPC → Subnets → Create Subnet

## Public Subnet

VPC
`Medical-Data-VPC`

Subnet Name
`Medical-Public-Subnet`

Availability Zone
`us-east-1a`

IPv4 CIDR Block
`192.168.0.1/27`

Create the subnet.

---

## Private Subnet

Create another subnet.

VPC
`Medical-Data-VPC`

Subnet Name
`Medical-Private-Subnet`

Availability Zone
`us-east-1b`

IPv4 CIDR Block
`192.168.0.32/27`

Create the subnet.

This subnet will host **internal medical processing systems**.

---

# Step 5 — Enable Public IP Assignment for Public Subnet

To allow administrators to access the Bastion Host, enable public IP assignment.

Navigate to:

Subnets → Select `Medical-Public-Subnet`

Actions → Edit subnet settings

Enable:

Auto-assign public IPv4 address

Save changes.

---

# Step 6 — Create Route Tables

Route tables determine how traffic flows within the VPC.

Navigate to:

VPC → Route Tables → Create route table

## Public Route Table

Name
`Medical-PublicRouteTable`

VPC
`Medical-Data-VPC`

Create the route table.

Associate it with:

`Medical-Public-Subnet`

Then configure the route:

Destination
`0.0.0.0/0`

Target
`Internet Gateway`

Select the gateway created earlier.

---

## Private Route Table

Create another route table.

Name
`Medical-PrivateRouteTable`

VPC
`Medical-Data-VPC`

Associate this route table with:

`Medical-Private-Subnet`

No internet route is added here.

The private subnet will remain **isolated from the internet**.

---

# Step 7 — Configure Security Groups

Two security groups are created to control access between infrastructure components.

---

## Bastion Host Security Group

Navigate to:

VPC → Security Groups → Create Security Group

Configuration:

Name
`Bastion-SG`

Description
Security group for administrative Bastion Host

VPC
`Medical-Data-VPC`

Inbound Rules:

SSH (22)
Source: `0.0.0.0/0`

HTTP (80)
Source: `0.0.0.0/0`

HTTPS (443)
Source: `0.0.0.0/0`

This allows administrators to securely connect to the Bastion Host.

---

## Private Processing Instance Security Group

Create another security group.

Name
`Medical-Endpoint-SG`

Description
Security group for private medical processing servers

VPC
`Medical-Data-VPC`

Inbound Rule:

SSH (22)

Source
`Bastion-SG`

This ensures that **only the Bastion Host can access the private instance**.

---

# Step 8 — Launch the Bastion Host

The Bastion Host acts as a secure entry point to the private infrastructure.

Navigate to:

Services → EC2 → Launch Instance

Configuration:

Name
`Medical-Bastion-Host`

AMI
Amazon Linux 2023 (kernel 6.1)

Instance Type
`t2.micro`

Key Pair
Create a key pair

Name
`MedicalAccessKey`

Network Settings:

VPC
`Medical-Data-VPC`

Subnet
`Medical-Public-Subnet`

Auto assign public IP
Enabled

Security Group
`Bastion-SG`

Launch the instance.

Wait until the instance status becomes **Running**.

---

# Step 9 — Launch the Private Medical Processing Instance

This instance simulates a backend server responsible for processing medical datasets stored in S3.

Launch another EC2 instance.

Configuration:

Name
`Medical-Processing-Server`

AMI
Amazon Linux 2023 (kernel 6.1)

Instance Type
`t2.micro`

Key Pair
`MedicalAccessKey`

Network Settings:

VPC
`Medical-Data-VPC`

Subnet
`Medical-Private-Subnet`

Security Group
`Medical-Endpoint-SG`

This instance **does not receive a public IP address**.

Launch the instance and wait until the status becomes **Running**.

---

# Step 10 — Access the Private Instance via Bastion Host

First connect to the Bastion Host from your local machine.

Then copy the private key file to the Bastion Host so it can be used to access internal servers.

Create a key file on the Bastion Host:

```
vi MedicalAccessKey.pem
```

Paste the key contents and save.

Then update permissions:

```
chmod 400 MedicalAccessKey.pem
```

Now SSH into the private medical processing instance using its private IP address.

---

# Step 11 — Create a VPC Endpoint for Amazon S3

The medical processing server must access datasets stored in Amazon S3.

Instead of using the internet, a **Gateway VPC Endpoint** will be created.

Navigate to:

VPC → Endpoints → Create Endpoint

Service Category
AWS Services

Search for:

`s3`

Select the service:

Gateway endpoint
`com.amazonaws.us-east-1.s3`

Configuration:

VPC
`Medical-Data-VPC`

Route Table
`Medical-PrivateRouteTable`

Create the endpoint.

This automatically adds a route allowing the private subnet to access S3 through the AWS network.

---

# Step 12 — Validate Secure Access to Medical Data Storage

Reconnect to the private processing instance through the Bastion Host.

Configure the AWS CLI:

```
aws configure
```

Provide credentials and region.

Now test S3 connectivity using the AWS CLI.

If the VPC endpoint is configured correctly, the private instance will successfully access S3 **without internet connectivity**.

This confirms that the secure architecture is functioning as expected.

---

# Implementation Result

The final architecture enables:

* Secure administrative access through a Bastion Host
* Private compute resources in an isolated subnet
* Secure communication between EC2 and S3 using a VPC Endpoint
* No exposure of sensitive medical data traffic to the public internet

This pattern is widely used in **healthcare, finance, and regulated industries** to protect sensitive data pipelines.
