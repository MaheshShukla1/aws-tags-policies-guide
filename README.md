# Introduction to Tags in AWS Policies
### Table of Contents
- [Detailed Explanations and Examples](#detailed-explanations-and-examples)
- [Types of Tags](#types-of-tags)
- [Common Tag Keys](#common-tag-keys)
- [Practical Examples](#practical-examples)
- [Example 1: EC2 Instance Creation Policy](#example-1-ec2-instance-creation-policy)
- [Example 2: EC2 Instance Management Policy](#example-2-ec2-instance-management-policy)
- [Example 3: S3 Object Management Policy](#example-3-s3-object-management-policy)
  
Tags are key-value pairs that provide metadata for AWS resources. They play a crucial role in managing and controlling access to resources in AWS, especially in Attribute-Based Access Control (ABAC) policies. Tags can be applied to requests, IAM principals, or resources and can be used in policy conditions to enforce access controls.

### Types of Tags

1. **Request Tags**: Tags specified at the time of resource creation or modification.
2. **Principal Tags**: Tags associated with IAM principals (users or roles).
3. **Resource Tags**: Tags applied to existing resources.

### Common Tag Keys

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

- **Description**: Used to specify tags applied at the request level. These tags are used when a resource is created or modified and can be used to control access based on the tags applied to a request.
- **Example**: `aws:RequestTag/Environment` can be used to enforce that only resources tagged with a specific environment tag can be created.

### 2. `aws:PrincipalTag/<key>`

- **Description**: Refers to tags associated with the IAM principal (user or role) making the request. Useful for ABAC (Attribute-Based Access Control) policies where access is granted based on the tags of the requesting principal.
- **Example**: `aws:PrincipalTag/Department` can be used to allow access only to users with a specific department tag.

### 3. `aws:ResourceTag/<key>`

- **Description**: Refers to tags associated with the resource being accessed or managed. This is used to enforce policies based on the tags of the resource.
- **Example**: `aws:ResourceTag/Project` can be used to allow access to resources only if they have a specific project tag.

### 4. `s3:ExistingObjectTag/<key>`

- **Description**: Used in Amazon S3 policies to refer to tags applied to existing objects within a bucket. Useful for policies that depend on object tags.
- **Example**: `s3:ExistingObjectTag/Environment` can be used to allow access based on the tags of existing S3 objects.

### 5. `s3:RequestObjectTag/<key>`

- **Description**: Used in Amazon S3 policies to refer to tags being requested for new objects. This is useful when controlling actions based on the tags specified during object creation.
- **Example**: `s3:RequestObjectTag/Environment` can be used to enforce that new objects must have specific tags.

### 6. `ec2:ResourceTag/<key>`

- **Description**: Refers to tags on EC2 resources (e.g., instances, volumes). Useful for policies that control access based on EC2 resource tags.
- **Example**: `ec2:ResourceTag/Environment` can be used to control access to EC2 instances based on their environment tag.

### 7. `ec2:RequestTag/<key>`

- **Description**: Refers to tags applied to EC2 resources at the time of creation. Useful for controlling resource creation based on tags specified during the request.
- **Example**: `ec2:RequestTag/Project` can be used to allow or deny the creation of EC2 instances based on the project tag provided in the request.

### 8. `rds:ResourceTag/<key>`

- **Description**: Refers to tags on RDS (Relational Database Service) resources (e.g., databases, instances). Useful for policies controlling access based on RDS resource tags.
- **Example**: `rds:ResourceTag/Environment` can be used to control access to RDS instances based on their environment tag.

### 9. `rds:RequestTag/<key>`

- **Description**: Refers to tags applied to RDS resources at creation time. Useful for controlling the creation of RDS resources based on tags specified in the request.
- **Example**: `rds:RequestTag/Project` can be used to ensure that only RDS instances with specific tags can be created.

### 10. `kms:RequestTag/<key>`

- **Description**: Refers to tags specified when creating or modifying AWS Key Management Service (KMS) keys. Useful for controlling key creation based on tags.
- **Example**: `kms:RequestTag/Project` can be used to enforce specific tags on KMS keys during their creation.

### 11. `kms:ResourceTag/<key>`

- **Description**: Refers to tags on KMS keys. Useful for policies that need to control access based on existing KMS key tags.
- **Example**: `kms:ResourceTag/Environment` can be used to allow or deny access to KMS keys based on their environment tag.

## Practical Examples

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
```
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

