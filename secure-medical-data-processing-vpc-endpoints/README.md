# Secure Medical Data Processing Infrastructure on AWS Using VPC Endpoints

## Project Overview

This project demonstrates how to design a **secure cloud architecture that allows a private EC2 instance to access Amazon S3 without traversing the public internet**.

The architecture implements a **VPC Gateway Endpoint for S3**, allowing secure and private communication between compute resources and cloud storage within the AWS network.

The design pattern is particularly relevant for **healthcare systems**, where sensitive medical data must be processed securely while maintaining strong network isolation.

This implementation was completed as part of a hands-on lab and expanded into a real-world architectural scenario for **secure medical data processing pipelines**.

---

## Real-World Scenario

Healthcare organizations frequently process large volumes of sensitive patient data such as:

* Electronic health records (EHR)
* Medical imaging datasets
* Laboratory results
* Diagnostic reports

A common challenge is enabling backend processing systems to access storage **without exposing the infrastructure to the public internet**.

If compute resources access storage through public endpoints, it can introduce:

* security vulnerabilities
* compliance risks
* increased attack surface

This project demonstrates how to design a **secure architecture where private compute resources access Amazon S3 entirely through the AWS internal network**.

---

## Architecture Overview

The architecture includes the following components:

* A custom Virtual Private Cloud
* Public and Private subnets
* A Bastion Host for secure administrative access
* A private EC2 instance responsible for data processing
* An S3 bucket for storing medical datasets
* A VPC Gateway Endpoint for private access to S3

Traffic between the EC2 instance and S3 **never leaves the AWS network**.

---

## Architecture Flow

1. An administrator connects to a Bastion Host located in the public subnet.
2. From the Bastion Host, the administrator securely SSHs into a private EC2 instance.
3. The private EC2 instance processes data and accesses an S3 bucket.
4. Access to S3 is provided through a VPC Gateway Endpoint instead of the internet.
5. All data transfers remain within the AWS infrastructure.

---

## AWS Services Used

* Amazon VPC
* Amazon EC2
* Amazon S3
* VPC Gateway Endpoint
* Internet Gateway
* Route Tables
* Security Groups

---

## Implementation Steps

### 1. Sign in to AWS Console

Logged into the AWS Management Console to begin infrastructure deployment.

---

### 2. Create a Virtual Private Cloud

A custom VPC was created to isolate the environment.

Configuration:

CIDR block:
`10.0.0.0/16`

This network space allows flexible subnet segmentation for future expansion.

---

### 3. Create and Attach an Internet Gateway

An Internet Gateway was created and attached to the VPC.

Purpose:

Allow controlled internet access for resources placed in the public subnet, particularly the Bastion Host.

---

### 4. Create Public and Private Subnets

Two subnets were created within the VPC.

Public Subnet

Hosts internet-facing resources such as the Bastion Host.

Private Subnet

Hosts internal compute resources that should not be publicly accessible.

---

### 5. Enable Public IPv4 Address Assignment for Public Subnet

The public subnet was configured to automatically assign public IP addresses to instances launched within it.

This ensures the Bastion Host can be accessed from the internet.

---

### 6. Create Route Table for the Public Subnet

A custom route table was created and associated with the public subnet.

Route configuration included:

Destination: `0.0.0.0/0`
Target: Internet Gateway

This enables internet connectivity for instances in the public subnet.

---

### 7. Create Security Groups

Security groups were configured to control traffic.

Example configuration:

Bastion Host Security Group

* Allow SSH from administrator IP

Private EC2 Security Group

* Allow SSH only from Bastion Host security group

This enforces controlled administrative access.

---

### 8. Launch the Bastion Host

An EC2 instance was launched in the **public subnet** to act as a Bastion Host.

Purpose:

Provide a secure entry point for administrators to access private infrastructure.

---

### 9. Launch the Private Endpoint Instance

A second EC2 instance was launched in the **private subnet**.

This instance simulates a backend **medical data processing server** responsible for accessing S3 datasets.

The instance has **no direct internet access**.

---

### 10. SSH into Private Instance via Bastion Host

Administrative access was performed using a **jump host model**.

Steps:

1. SSH into Bastion Host
2. From Bastion Host, SSH into the private EC2 instance

This ensures the private instance remains inaccessible from the internet.

---

### 11. Create a VPC Endpoint for S3

A **Gateway VPC Endpoint for S3** was created.

The endpoint was associated with the **private subnet route table**.

This configuration allows the private EC2 instance to communicate with S3 **without requiring internet connectivity**.

---

### 12. Access S3 from the Private EC2 Instance

From the private EC2 instance, AWS CLI commands were used to:

* List S3 buckets
* Access objects stored in the bucket

All communication occurred through the VPC endpoint.

---

### 13. Validate the Architecture

The final validation confirmed:

* Private EC2 instance could access S3
* No internet gateway was required for S3 access
* Communication occurred through the VPC endpoint

This confirmed that the architecture successfully implemented **private service connectivity**.

---

## Security Advantages

This architecture provides several security benefits.

### Private Service Access

Traffic between EC2 and S3 never traverses the public internet.

---

### Reduced Attack Surface

No external endpoints are exposed for the data processing instance.

---

### Secure Administrative Access

The Bastion Host pattern ensures that private instances remain isolated while still manageable.

---

### Compliance-Friendly Architecture

This design aligns with security practices often required for regulated environments such as healthcare systems.

---

## Key Cloud Architecture Concepts Demonstrated

* Secure network segmentation
* Bastion host architecture
* Private service access using VPC endpoints
* Infrastructure isolation within a VPC
* Secure access to cloud storage

---

## Related Blog Article

This project is further explained in the following article:

[Designing Secure Medical Data Processing Infrastructure on AWS Using VPC Endpoints
](https://architectwithmelano.hashnode.dev/designing-secure-medical-data-processing-infrastructure-on-aws-using-vpc-endpoints)

The article explores the **architectural thinking and real-world application** of the solution.

---

## Future Improvements

Potential enhancements to this architecture include:

* IAM role-based access control for EC2
* S3 bucket policies restricting access to the VPC endpoint
* Encryption using AWS KMS
* Monitoring with CloudWatch
* Logging using AWS CloudTrail

---

## Author

Emmanuel Essien

Aspiring Cloud Solutions Architect with interests in:

Cloud Infrastructure
AI-powered systems
Data analytics platforms
Secure system architecture

---
