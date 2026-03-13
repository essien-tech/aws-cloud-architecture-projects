# Learning Resources

This document lists the key learning materials and references used while building the **Secure Medical Data Processing Infrastructure using VPC Endpoints** project.

These resources helped guide the architecture design, security decisions, and AWS implementation.

---

# Hands-On Lab Resource

The foundational lab used for building the infrastructure was provided by Whizlabs.

It demonstrated how a private EC2 instance can securely access Amazon S3 using a VPC endpoint without requiring internet connectivity.

Lab Title
Access S3 from Private EC2 Instance Using VPC Endpoint

[Link 
](https://business.whizlabs.com/labs/access-s3-from-private-ec2-instance-using-vpc-endpoint)

This lab served as the technical foundation for the project and was expanded into a real-world architecture scenario focused on **secure healthcare data processing infrastructure**.

---

# Project Blog Article

A detailed explanation of the architectural reasoning behind this project can be found in the accompanying blog article.

Blog Title
[Designing Secure Medical Data Processing Infrastructure on AWS Using VPC Endpoints
](https://architectwithmelano.hashnode.dev/designing-secure-medical-data-processing-infrastructure-on-aws-using-vpc-endpoints)

The article explains:

• the real-world problem

• the architecture pattern used

• security considerations

• practical use cases in healthcare systems

---

# AWS Documentation

The following official AWS documentation was used to understand the services involved in this architecture.

## Amazon VPC Documentation

The Amazon VPC documentation explains how to design isolated virtual networks and configure subnets, route tables, and internet gateways.

Topics studied include:

• creating custom VPC networks

• subnet segmentation

• route table configuration

• network isolation strategies

---

## Amazon EC2 Documentation

The EC2 documentation helped guide the deployment and configuration of the Bastion Host and private processing instances.

Topics studied include:

• instance launch configuration

• key pair authentication

• security groups

• remote administration using SSH

---

## Amazon S3 Documentation

The S3 documentation provided guidance on interacting with object storage and accessing data through the AWS CLI.

Topics studied include:

• bucket structure

• object listing and retrieval

• CLI interaction with S3

---

## VPC Endpoint Documentation

Understanding VPC endpoints was essential for designing secure private access to S3.

Topics studied include:

• gateway endpoints vs interface endpoints

• routing traffic through private endpoints

• secure service access without internet connectivity

---

# Additional Learning Topics

While building this project, several related cloud architecture concepts were explored.

These include:

• Bastion host architecture patterns

• network segmentation in cloud environments

• secure data pipelines

• private service connectivity in AWS

These concepts are widely used when designing infrastructure for **regulated industries such as healthcare and finance**.

---

# Future Learning Directions

To further strengthen this architecture, the following AWS topics will be explored:

• IAM roles for EC2 instances

• S3 bucket policies with VPC endpoint restrictions

• encryption with AWS KMS

• centralized monitoring using CloudWatch

• logging and auditing using CloudTrail

These additions will help transform the current architecture into a **production-ready secure data processing platform**.

---

# Summary

The resources above helped build a deeper understanding of how to design secure cloud architectures that protect sensitive data.

This project represents a practical application of those concepts using AWS infrastructure services.
