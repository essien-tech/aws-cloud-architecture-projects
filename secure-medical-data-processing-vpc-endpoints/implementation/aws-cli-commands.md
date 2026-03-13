# AWS CLI Commands Used in the Project

This document lists the AWS CLI commands used to validate connectivity between the **private medical processing server** and the secure storage layer in Amazon S3.

These commands were executed from the **private EC2 instance** after connecting through the Bastion Host.

---

# Configure AWS CLI

Before interacting with AWS services, configure the AWS CLI with appropriate credentials.

```
aws configure
```

You will be prompted to enter:

Access Key ID

Secret Access Key

Default region name

```
us-east-1
```

Default output format can be left empty.

---

# Verify S3 Connectivity

Once the CLI is configured, verify that the private instance can communicate with Amazon S3 through the VPC endpoint.

List all S3 buckets accessible to the account:

```
aws s3 ls
```

Successful output confirms that the instance can reach S3.

---

# List Objects in a Specific Bucket

To inspect the contents of a bucket used for storing datasets:

```
aws s3 ls s3://bucket-name
```

Example:

```
aws s3 ls s3://medical-datasets
```

This command lists all objects stored in the specified bucket.

---

# Example Output

Typical output may look like:

```
2024-01-10 14:21:33   245120 patient-records.csv
2024-01-10 14:22:11   932480 imaging-data.zip
2024-01-10 14:23:45    10240 lab-results.json
```

This confirms that the private EC2 instance successfully accessed the storage service.

---

# Validation of the Architecture

Successful execution of these commands confirms that:

* The EC2 instance in the private subnet can access S3
* The communication occurs through the VPC Endpoint
* No public internet connectivity is required

This validates the **secure architecture design for sensitive medical data processing systems**.
