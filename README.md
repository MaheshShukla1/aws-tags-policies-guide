# Introduction to Tags in AWS Policies

## Table of Contents

- [Overview of AWS Tags](#overview-of-aws-tags)
- [Types of Tags in AWS](#types-of-tags-in-aws)
- [Common Tag Keys](#common-tag-keys)
- [Detailed Explanations and Examples](#detailed-explanations-and-examples)
    - [1. aws
        
        /<key>](#1-awsrequesttagkey)
    - [2. aws
        
        /<key>](#2-awsprincipaltagkey)
    - [3. aws
        
        /<key>](#3-awsresourcetagkey)
    - [4. s3
        
        /<key>](#4-s3existingobjecttagkey)
    - [5. s3
        
        /<key>](#5-s3requestobjecttagkey)
    - [6. ec2
        
        /<key>](#6-ec2resourcetagkey)
    - [7. ec2
        
        /<key>](#7-ec2requesttagkey)
    - [8. rds
        
        /<key>](#8-rdsresourcetagkey)
    - [9. rds
        
        /<key>](#9-rdsrequesttagkey)
    - [10. kms
        
        /<key>](#10-kmsrequesttagkey)
    - [11. kms
        
        /<key>](#11-kmsresourcetagkey)
- [Practical Use Cases](#practical-use-cases)
    - [Example 1: EC2 Instance Creation Policy](#example-1-ec2-instance-creation-policy)
    - [Example 2: EC2 Instance Management Policy](#example-2-ec2-instance-management-policy)
    - [Example 3: S3 Object Management Policy](#example-3-s3-object-management-policy)

## Overview of AWS Tags

Tags in AWS are key-value pairs that provide metadata for AWS resources, playing a crucial role in managing and controlling access to these resources. Tags are essential in Attribute-Based Access Control (ABAC) policies, allowing for granular control based on attributes rather than traditional identity-based permissions. Tags can be applied to requests, IAM principals (users or roles), and resources, enabling more flexible and scalable security practices.

## Types of Tags in AWS

1. **Request Tags**: Tags applied at the time of resource creation or modification.
2. **Principal Tags**: Tags associated with IAM principals (users or roles).
3. **Resource Tags**: Tags applied to existing resources within AWS.

## Common Tag Keys

1. `aws:RequestTag/<key>`
2. `aws:PrincipalTag/<key>`
3. `aws:ResourceTag/<key>`
4. `s3:ExistingObjectTag/<key>`
5. `s3:RequestObjectTag/<key>`
6. `ec2:RequestTag/<key>`
7. `ec2:ResourceTag/<key>`
8. `rds:RequestTag/<key>`
9. `rds:ResourceTag/<key>`
10. `kms:RequestTag/<key>`
11. `kms:ResourceTag/<key>`

## Detailed Explanations and Examples

### 1. `aws:RequestTag/<key>`

- **Description**: Tags specified at the request level, applied during the creation or modification of a resource. These tags are pivotal for controlling access based on conditions set in the request.
- **Example**: The tag `aws:RequestTag/Environment` can be used to enforce that only resources tagged with a specific environment value can be created.

### 2. `aws:PrincipalTag/<key>`

- **Description**: Tags associated with the IAM principal (user or role) making the request. These are particularly useful in ABAC (Attribute-Based Access Control) policies, enabling access based on the tags of the requesting principal.
- **Example**: The tag `aws:PrincipalTag/Department` can be used to grant access only to users with a specific department tag.

### 3. `aws:ResourceTag/<key>`

- **Description**: Tags associated with the resource being accessed or managed. These tags help enforce policies based on the resource’s attributes.
- **Example**: The tag `aws:ResourceTag/Project` can be used to grant access to resources only if they have a specific project tag.

### 4. `s3:ExistingObjectTag/<key>`

- **Description**: Used in Amazon S3 policies to refer to tags applied to existing objects within a bucket. This is crucial for policies that depend on object-specific tags.
- **Example**: The tag `s3:ExistingObjectTag/Environment` can be used to allow access based on the tags of existing S3 objects.

### 5. `s3:RequestObjectTag/<key>`

- **Description**: Tags specified for new objects in an Amazon S3 bucket. These tags help enforce conditions during the creation of new objects.
- **Example**: The tag `s3:RequestObjectTag/Environment` can be used to ensure that new objects must have specific tags.

### 6. `ec2:ResourceTag/<key>`

- **Description**: Tags applied to EC2 resources (e.g., instances, volumes). These are critical for policies controlling access based on EC2 resource tags.
- **Example**: The tag `ec2:ResourceTag/Environment` can be used to control access to EC2 instances based on their environment tag.

### 7. `ec2:RequestTag/<key>`

- **Description**: Tags applied to EC2 resources during their creation. These tags are essential for controlling resource creation based on tags specified in the request.
- **Example**: The tag `ec2:RequestTag/Project` can be used to permit or deny the creation of EC2 instances based on the project tag.

### 8. `rds:ResourceTag/<key>`

- **Description**: Tags associated with RDS (Relational Database Service) resources. These tags help enforce policies that control access to RDS resources based on their tags.
- **Example**: The tag `rds:ResourceTag/Environment` can be used to control access to RDS instances based on their environment tag.

### 9. `rds:RequestTag/<key>`

- **Description**: Tags applied to RDS resources at the time of creation. These are essential for controlling the creation of RDS resources based on tags in the request.
- **Example**: The tag `rds:RequestTag/Project` can be used to ensure that RDS instances are created only with specific project tags.

### 10. `kms:RequestTag/<key>`

- **Description**: Tags specified during the creation or modification of AWS Key Management Service (KMS) keys. These tags are critical for controlling the creation of keys based on specified tags.
- **Example**: The tag `kms:RequestTag/Project` can be used to enforce specific tags on KMS keys during their creation.

### 11. `kms:ResourceTag/<key>`

- **Description**: Tags associated with KMS keys. These are crucial for policies that control access based on the existing tags of KMS keys.
- **Example**: The tag `kms:ResourceTag/Environment` can be used to allow or deny access to KMS keys based on their environment tag.

## Practical Use Cases

### Example 1: EC2 Instance Creation Policy

This policy allows users with the `Development` department tag to create EC2 instances with the `Development` environment tag.


```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:RunInstances",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalTag/Department": "Development",
                    "ec2:RequestTag/Environment": "Development"
                }
            }
        }
    ]
}
````

### Example 2: EC2 Instance Management Policy

This policy allows users with the `Production` department tag to start, stop, and reboot EC2 instances that are tagged with the `Production` environment tag.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:StartInstances",
                "ec2:StopInstances",
                "ec2:RebootInstances"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalTag/Department": "Production",
                    "ec2:ResourceTag/Environment": "Production"
                }
            }
        }
    ]
}
```

### Example 3: S3 Object Management Policy

This policy allows users to get and put objects in an S3 bucket if they have the `Development` environment tag on the principal and the existing objects have the `Production` environment tag.


```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::s3bucketslearningproduction/*",
            "Condition": {
                "StringEquals": {
                    "aws:PrincipalTag/Environment": "Development",
                    "s3:ExistingObjectTag/Environment": "Production"
                }
            }
        }
    ]
}
```


## Conclusion

Tags are a powerful and flexible way to manage and control access to AWS resources. By leveraging tags in IAM policies, you can implement more granular and effective security controls that align with your organization’s operational and compliance needs.

This guide has provided an in-depth look at how to use tags effectively in AWS IAM policies, complete with practical examples. Whether you're just getting started or looking to refine your AWS security practices, understanding and using tags will enhance your ability to manage and secure your AWS environment.
