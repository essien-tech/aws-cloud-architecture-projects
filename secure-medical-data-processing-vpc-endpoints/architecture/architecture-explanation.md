# Architecture Explanation

## Secure Medical Data Processing Infrastructure

This document explains the architecture used to implement a **secure medical data processing environment** using AWS networking and storage services.

The goal of the architecture is to ensure that sensitive medical datasets stored in Amazon S3 can be accessed **securely and privately by internal compute resources** without exposing data traffic to the public internet.

---

# Architectural Design Principles

The architecture was designed using the following cloud security principles:

• Network isolation

• Least privilege access

• Controlled administrative access

• Private service communication

• Reduced attack surface

These principles are critical for systems that process **sensitive healthcare data**.

---

# Core Architecture Components

## Virtual Private Cloud (VPC)

A custom VPC provides a logically isolated network environment where all infrastructure resources operate.

The VPC CIDR block:
192.168.0.0/26

This address space allows the network to be segmented into multiple subnets for workload isolation.

---

## Public Subnet

The public subnet hosts a Bastion Host that administrators use to securely access internal systems.

Characteristics:

• Connected to the Internet Gateway

• Allows public IP assignment

• Used only for administrative access

No sensitive workloads are deployed here.

---

## Private Subnet

The private subnet hosts backend compute resources responsible for processing medical datasets.

Characteristics:

• No direct internet access

• Not assigned public IP addresses

• Access controlled through security groups

This subnet contains the **medical data processing server**.

---

## Bastion Host

The Bastion Host is a publicly accessible EC2 instance used as a secure administrative entry point into the private network.

Administrators connect to the Bastion Host first, then use it as a jump server to access private infrastructure.

Benefits:

• Prevents exposing private instances to the internet

• Centralizes administrative access

• Enables strong access control policies

---

## Private Medical Processing Instance

This EC2 instance represents a backend application responsible for processing medical datasets.

Examples of workloads it may perform:

• medical image processing

• patient record analysis

• laboratory data processing

• analytics workloads

This instance runs entirely inside the private subnet.

---

## Secure Storage Layer

Medical datasets are stored in Amazon S3.

Typical examples of stored data:

• electronic health records

• diagnostic imaging files

• patient lab results

• anonymized medical research datasets

The storage layer must be accessed securely without exposing network traffic externally.

---

## VPC Gateway Endpoint for S3

A Gateway VPC Endpoint enables private connectivity between the VPC and Amazon S3.

Instead of sending requests to the public S3 endpoint through the internet, traffic is routed internally within the AWS network.

Benefits:

• No internet gateway required

• No NAT gateway required

• Data remains inside the AWS network

• Improved security posture

The endpoint is attached to the **private route table**, allowing the private subnet to access S3 securely.

---

# Traffic Flow

The system operates using the following access flow.

1. Administrator connects to Bastion Host using SSH.
2. Administrator connects from Bastion Host to the private processing server.
3. The private processing server retrieves data from Amazon S3.
4. Communication between EC2 and S3 occurs through the VPC Endpoint.

At no point does medical data traffic traverse the public internet.

---

# Architecture Outcome

The final architecture ensures that:

• Administrative access is controlled

• Compute resources are isolated

• Storage access remains private

• Sensitive healthcare data is protected

This design pattern is commonly used in **regulated industries such as healthcare, finance, and government infrastructure**.
