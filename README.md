# 🚀 LIVE EXAMPLES - AWS IAM ENUMERATION COMMANDS!

## 🎨 COLOR CODE:
- 🔴 **RED** = CRITICAL INFORMATION TO LOOK FOR
- 🟡 **YELLOW** = Important Values (ARNs, Names, IDs)
- 🟢 **GREEN** = Success/Found Items
- 🔵 **BLUE** = Commands You Type

---

# 📋 PART A: USER ENUMERATION

## 🔵 1. List All IAM Users
```bash
aws iam list-users
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Users": [
        {
            🔴 "UserName": "BackupReader1",
            🔴 "UserId": "AIDAQ3EGUZME24ILVB44T",
            🔴 "Arn": "arn:aws:iam::058264439561:user/BackupReader1",
            "CreateDate": "2024-09-24T11:12:45+00:00",
            "PasswordLastUsed": "2025-03-10T08:15:23+00:00"
        },
        {
            🔴 "UserName": "AdminUser",
            🔴 "UserId": "AIDAQ3EGUZME5HY78JKL",
            🔴 "Arn": "arn:aws:iam::058264439561:user/AdminUser",
            "CreateDate": "2024-01-15T09:30:22+00:00"
        },
        {
            🔴 "UserName": "Developer1",
            🔴 "UserId": "AIDAQ3EGUZME9KJ78H5G",
            🔴 "Arn": "arn:aws:iam::058264439561:user/Developer1",
            "CreateDate": "2024-06-20T14:22:10+00:00"
        },
        {
            🔴 "UserName": "S3Reader",
            🔴 "UserId": "AIDAQ3EGUZME3N5T7R2E",
            🔴 "Arn": "arn:aws:iam::058264439561:user/S3Reader",
            "CreateDate": "2024-11-05T10:30:00+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **UserNames** - Who has access?
- 🔴 **ARNs** - Full resource names
- 🟡 **CreateDate** - When accounts created
- 🟡 **PasswordLastUsed** - Active users

---

## 🔵 2. List Groups for Specific User
```bash
aws iam list-groups-for-user --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Groups": [
        {
            🔴 "GroupName": "Developers",
            🔴 "GroupId": "AGPAQ3EGUZME8B9C0D1E",
            🔴 "Arn": "arn:aws:iam::058264439561:group/Developers",
            "CreateDate": "2024-03-15T10:20:00+00:00"
        },
        {
            🔴 "GroupName": "S3Access",
            🔴 "GroupId": "AGPAQ3EGUZME2F4G6H8J",
            🔴 "Arn": "arn:aws:iam::058264439561:group/S3Access",
            "CreateDate": "2024-05-10T09:15:30+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **GroupNames** - What groups user belongs to
- 🔴 **Group ARNs** - For further enumeration

---

## 🔵 3. List Managed Policies Attached to User
```bash
aws iam list-attached-user-policies --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "AttachedPolicies": [
        {
            🔴 "PolicyName": "PermissionBoundaryPolicy",
            🔴 "PolicyArn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy"
        },
        {
            🔴 "PolicyName": "UserPolicy",
            🔴 "PolicyArn": "arn:aws:iam::058264439561:policy/UserPolicy"
        },
        {
            🔴 "PolicyName": "S3ReadOnlyAccess",
            🔴 "PolicyArn": "arn:aws:iam::aws:policy/S3ReadOnlyAccess"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **PolicyNames** - What policies are attached
- 🔴 **PolicyArns** - Note AWS managed vs custom policies
- 🟢 **"arn:aws:iam::aws:policy/"** = AWS managed (less interesting)
- 🔴 **"arn:aws:iam::058264439561:policy/"** = Custom policies (more interesting!)

---

## 🔵 4. List Inline Policies for User
```bash
aws iam list-user-policies --user-name BackupReader1
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "PolicyNames": [
        🔴 "KMSFullAccessPolicy",
        🔴 "CustomS3Policy",
        🔴 "DebugAccessPolicy"
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **PolicyNames** - Custom policies just for this user
- 🟡 These are often over-privileged!

---

# 📋 PART B: GROUP ENUMERATION

## 🔵 1. List All IAM Groups
```bash
aws iam list-groups
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Groups": [
        {
            🔴 "GroupName": "Admins",
            🔴 "GroupId": "AGPAQ3EGUZME4X5Y6Z7A",
            🔴 "Arn": "arn:aws:iam::058264439561:group/Admins",
            "CreateDate": "2024-01-01T00:00:00+00:00"
        },
        {
            🔴 "GroupName": "Developers",
            🔴 "GroupId": "AGPAQ3EGUZME8B9C0D1E",
            🔴 "Arn": "arn:aws:iam::058264439561:group/Developers",
            "CreateDate": "2024-03-15T10:20:00+00:00"
        },
        {
            🔴 "GroupName": "Auditors",
            🔴 "GroupId": "AGPAQ3EGUZME1A2B3C4D",
            🔴 "Arn": "arn:aws:iam::058264439561:group/Auditors",
            "CreateDate": "2024-07-22T13:45:00+00:00"
        },
        {
            🔴 "GroupName": "S3Access",
            🔴 "GroupId": "AGPAQ3EGUZME2F4G6H8J",
            🔴 "Arn": "arn:aws:iam::058264439561:group/S3Access",
            "CreateDate": "2024-05-10T09:15:30+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **GroupNames** - Identify high-privilege groups (Admins, etc.)
- 🔴 **ARNs** - For further enumeration

---

## 🔵 2. List Managed Policies Attached to Group
```bash
aws iam list-attached-group-policies --group-name Developers
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "AttachedPolicies": [
        {
            🔴 "PolicyName": "DeveloperPolicy",
            🔴 "PolicyArn": "arn:aws:iam::058264439561:policy/DeveloperPolicy"
        },
        {
            🔴 "PolicyName": "ReadOnlyAccess",
            🔴 "PolicyArn": "arn:aws:iam::aws:policy/ReadOnlyAccess"
        },
        {
            🔴 "PolicyName": "CloudWatchLogsFullAccess",
            🔴 "PolicyArn": "arn:aws:iam::aws:policy/CloudWatchLogsFullAccess"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Custom policies** (with account ID in ARN) - Most interesting!
- 🟡 AWS managed policies - Less likely to be misconfigured

---

## 🔵 3. List Inline Policies for Group
```bash
aws iam list-group-policies --group-name Developers
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "PolicyNames": [
        🔴 "EC2RestartPolicy",
        🔴 "S3WritePolicy",
        🔴 "TempAdminAccess"
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Inline policies** - Directly embedded in group
- 🟡 Names like "TempAdminAccess" are suspicious!

---

# 📋 PART C: ROLE ENUMERATION

## 🔵 1. List All IAM Roles
```bash
aws iam list-roles
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Roles": [
        {
            🔴 "RoleName": "LambdaExecutionRole",
            🔴 "RoleId": "AROAQ3EGUZME12345",
            🔴 "Arn": "arn:aws:iam::058264439561:role/LambdaExecutionRole",
            "CreateDate": "2024-02-10T11:30:22+00:00",
            "AssumeRolePolicyDocument": {
                "Statement": [
                    {
                        "Principal": {
                            🔴 "Service": "lambda.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        },
        {
            🔴 "RoleName": "EC2InstanceRole",
            🔴 "RoleId": "AROAQ3EGUZME67890",
            🔴 "Arn": "arn:aws:iam::058264439561:role/EC2InstanceRole",
            "CreateDate": "2024-04-15T09:20:45+00:00",
            "AssumeRolePolicyDocument": {
                "Statement": [
                    {
                        "Principal": {
                            🔴 "Service": "ec2.amazonaws.com"
                        },
                        "Action": "sts:AssumeRole"
                    }
                ]
            }
        },
        {
            🔴 "RoleName": "CrossAccountAccessRole",
            🔴 "RoleId": "AROAQ3EGUZME54321",
            🔴 "Arn": "arn:aws:iam::058264439561:role/CrossAccountAccessRole",
            "CreateDate": "2024-08-05T14:10:33+00:00",
            "AssumeRolePolicyDocument": {
                "Statement": [
                    {
                        "Principal": {
                            🔴 "AWS": "arn:aws:iam::123456789012:root"
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
- 🔴 **RoleNames** - Identify interesting roles
- 🔴 **AssumeRolePolicyDocument** - WHO can assume this role?
  - 🟡 `"Service": "lambda.amazonaws.com"` - Lambda can assume
  - 🟡 `"Service": "ec2.amazonaws.com"` - EC2 can assume
  - 🔴 `"AWS": "arn:aws:iam::123456789012:root"` - Cross-account access!

---

## 🔵 2. List Managed Policies Attached to Role
```bash
aws iam list-attached-role-policies --role-name LambdaExecutionRole
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "AttachedPolicies": [
        {
            🔴 "PolicyName": "LambdaBasicExecution",
            🔴 "PolicyArn": "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole"
        },
        {
            🔴 "PolicyName": "S3FullAccess",
            🔴 "PolicyArn": "arn:aws:iam::058264439561:policy/S3FullAccess"
        },
        {
            🔴 "PolicyName": "DynamoDBReadOnly",
            🔴 "PolicyArn": "arn:aws:iam::aws:policy/DynamoDBReadOnlyAccess"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Custom policies** - The dangerous ones!
- 🔴 **S3FullAccess** on Lambda role? That's interesting!

---

## 🔵 3. List Inline Policies for Role
```bash
aws iam list-role-policies --role-name LambdaExecutionRole
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "PolicyNames": [
        🔴 "SecretAccessPolicy",
        🔴 "KMSDecryptPolicy",
        🔴 "SQSWritePolicy"
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **SecretAccessPolicy** - Might access Secrets Manager!
- 🔴 **KMSDecryptPolicy** - Can decrypt data!

---

# 📋 PART D: POLICY ENUMERATION

## 🔵 1. List All IAM Policies
```bash
aws iam list-policies --scope Local --max-items 10
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Policies": [
        {
            🔴 "PolicyName": "PermissionBoundaryPolicy",
            🔴 "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
            🔴 "DefaultVersionId": "v3",
            "AttachmentCount": 0,
            "IsAttachable": true,
            "CreateDate": "2024-09-24T11:13:35+00:00"
        },
        {
            🔴 "PolicyName": "UserPolicy",
            🔴 "PolicyId": "ANPAQ3EGUZMEYBBY6PGTS",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/UserPolicy",
            🔴 "DefaultVersionId": "v1",
            "AttachmentCount": 1,
            "CreateDate": "2024-09-24T11:13:22+00:00"
        },
        {
            🔴 "PolicyName": "S3FullAccess",
            🔴 "PolicyId": "ANPAQ3EGUZME7H8J9K0L",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/S3FullAccess",
            🔴 "DefaultVersionId": "v2",
            "AttachmentCount": 3,
            "CreateDate": "2024-05-10T09:15:30+00:00"
        },
        {
            🔴 "PolicyName": "KMSAdminPolicy",
            🔴 "PolicyId": "ANPAQ3EGUZME1B2C3D4E",
            🔴 "Arn": "arn:aws:iam::058264439561:policy/KMSAdminPolicy",
            🔴 "DefaultVersionId": "v1",
            "AttachmentCount": 2,
            "CreateDate": "2024-07-22T13:45:00+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **PolicyNames** - Identify interesting policies
- 🔴 **DefaultVersionId** - Which version to check!
- 🟡 **AttachmentCount** - How widely used

---

## 🔵 2. Get Specific Policy Details
```bash
aws iam get-policy --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Policy": {
        🔴 "PolicyName": "PermissionBoundaryPolicy",
        🔴 "PolicyId": "ANPAQ3EGUZMEQVY32A5L4",
        🔴 "Arn": "arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy",
        🔴 "DefaultVersionId": "v3",
        "AttachmentCount": 0,
        "PermissionsBoundaryUsageCount": 1,
        "IsAttachable": true,
        "Description": "Permission boundary for the IAM user",
        "CreateDate": "2024-09-24T11:13:35+00:00",
        "UpdateDate": "2025-05-28T08:01:14+00:00"
    }
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **DefaultVersionId** - Remember this for next command!
- 🟡 **PermissionsBoundaryUsageCount** - How many users have this boundary

---

## 🔵 3. List All Versions of a Policy
```bash
aws iam list-policy-versions --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "Versions": [
        {
            🔴 "VersionId": "v3",
            "IsDefaultVersion": true,
            "CreateDate": "2025-05-28T08:01:14+00:00"
        },
        {
            🔴 "VersionId": "v2",
            "IsDefaultVersion": false,
            "CreateDate": "2024-12-15T10:30:22+00:00"
        },
        {
            🔴 "VersionId": "v1",
            "IsDefaultVersion": false,
            "CreateDate": "2024-09-24T11:13:35+00:00"
        }
    ]
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **All VersionIds** - Old versions might have more permissions!
- 🟡 **IsDefaultVersion** - Current active version
- 🔴 Check OLD versions for privilege escalation!

---

## 🔵 4. Get Specific Policy Version - 🔴 **MOST IMPORTANT!**
```bash
aws iam get-policy-version --policy-arn arn:aws:iam::058264439561:policy/PermissionBoundaryPolicy --version-id v3
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
```json
{
    "PolicyVersion": {
        "Document": {
            "Statement": [
                {
                    "Action": [
                        🔴 "s3:ListBucket",
                        🔴 "s3:GetObject"
                    ],
                    "Effect": "Allow",
                    "Resource": [
                        🔴 "arn:aws:s3:::securecopbakupbuk1",
                        🔴 "arn:aws:s3:::securecopbakupbuk1/*"
                    ]
                },
                {
                    "Action": [
                        🔴 "kms:Decrypt"
                    ],
                    "Effect": "Allow",
                    "Resource": 🔴 "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                },
                {
                    "Action": [
                        🔴 "iam:PutUserPolicy",
                        🔴 "iam:GetUser",
                        🔴 "iam:ListUserPolicies"
                    ],
                    "Effect": "Allow",
                    "Resource": 🔴 "arn:aws:iam::058264439561:user/BackupReader1"
                },
                {
                    "Action": [
                        🔴 "secretsmanager:GetSecretValue",
                        🔴 "secretsmanager:ListSecrets"
                    ],
                    "Effect": "Allow",
                    "Resource": "*"
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
- 🔴 **"Action"** - What actions are allowed?
  - `"*"` = Full access to everything! (DANGER!)
  - `"iam:PutUserPolicy"` = Can escalate privileges!
  - `"kms:Decrypt"` = Can decrypt data!
  - `"secretsmanager:GetSecretValue"` = Can steal secrets!
- 🔴 **"Resource"** - Which resources?
  - `"*"` = All resources! (DANGER!)
  - Specific bucket names, key ARNs, user ARNs

---

## 🔵 5. Get User Inline Policy
```bash
aws iam get-user-policy --user-name BackupReader1 --policy-name KMSFullAccessPolicy
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
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
                    🔴 "s3:*",
                    🔴 "kms:*"
                ],
                "Resource": [
                    🔴 "arn:aws:s3:::securecopbakupbuk1",
                    🔴 "arn:aws:s3:::securecopbakupbuk1/*",
                    🔴 "arn:aws:kms:us-east-1:058264439561:key/a7a251b3-c889-4dd1-9176-931c207c33d5"
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Action: "s3:*"** - Full S3 access!
- 🔴 **Action: "kms:*"** - Full KMS access!
- 🔴 **Resources** - Specific buckets and keys

---

## 🔵 6. Get Group Inline Policy
```bash
aws iam get-group-policy --group-name Developers --policy-name EC2RestartPolicy
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
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
                    🔴 "ec2:StartInstances",
                    🔴 "ec2:StopInstances",
                    🔴 "ec2:RebootInstances"
                ],
                "Resource": [
                    🔴 "arn:aws:ec2:us-east-1:058264439561:instance/*"
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Can start/stop EC2 instances** - Could cause DoS
- 🔴 **Resource: "*"** - All instances!

---

## 🔵 7. Get Role Inline Policy
```bash
aws iam get-role-policy --role-name LambdaExecutionRole --policy-name SecretAccessPolicy
```

### 🔴 **LIVE OUTPUT EXAMPLE:**
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
                    🔴 "secretsmanager:GetSecretValue",
                    🔴 "secretsmanager:ListSecrets"
                ],
                "Resource": [
                    🔴 "arn:aws:secretsmanager:us-east-1:058264439561:secret:database*"
                ]
            }
        ]
    }
}
```

### ✅ **WHAT TO CHECK:**
- 🔴 **Can access Secrets Manager** - Could steal credentials!
- 🔴 **Resource pattern** - `database*` means all database secrets!

---

# 📊 QUICK REFERENCE TABLE

| Command | Purpose | 🔴 Look For |
|---------|---------|-------------|
| `aws iam list-users` | Find all users | UserNames, ARNs |
| `aws iam list-groups-for-user` | User's groups | GroupNames |
| `aws iam list-attached-user-policies` | Managed policies for user | PolicyArns |
| `aws iam list-user-policies` | Inline policies for user | PolicyNames |
| `aws iam list-groups` | Find all groups | GroupNames |
| `aws iam list-attached-group-policies` | Managed policies for group | PolicyArns |
| `aws iam list-group-policies` | Inline policies for group | PolicyNames |
| `aws iam list-roles` | Find all roles | RoleNames, AssumeRolePolicy |
| `aws iam list-attached-role-policies` | Managed policies for role | PolicyArns |
| `aws iam list-role-policies` | Inline policies for role | PolicyNames |
| `aws iam list-policies` | Find all custom policies | PolicyNames, DefaultVersionId |
| `aws iam get-policy` | Get policy details | DefaultVersionId |
| `aws iam list-policy-versions` | Find old policy versions | VersionIds |
| 🔴 `aws iam get-policy-version` | **SEE ACTUAL PERMISSIONS!** | Actions, Resources |
| 🔴 `aws iam get-user-policy` | **SEE USER INLINE POLICY!** | Actions, Resources |
| 🔴 `aws iam get-group-policy` | **SEE GROUP INLINE POLICY!** | Actions, Resources |
| 🔴 `aws iam get-role-policy` | **SEE ROLE INLINE POLICY!** | Actions, Resources |

---

# 🏁 ENUMERATION CHEAT SHEET

## 🔴 HIGH-VALUE TARGETS TO LOOK FOR:

### Dangerous Actions:
```
🔴 "Action": "*"                                   (Full access to everything!)
🔴 "Action": "iam:*"                                (Can modify IAM)
🔴 "Action": "iam:PutUserPolicy"                    (Can escalate privileges!)
🔴 "Action": "iam:CreatePolicyVersion"              (Can change policies!)
🔴 "Action": "s3:*"                                  (Full S3 access)
🔴 "Action": "kms:*"                                 (Full KMS access)
🔴 "Action": "secretsmanager:GetSecretValue"        (Can steal secrets!)
🔴 "Action": "ssm:GetParameter*"                     (Can get parameters)
🔴 "Action": "ec2:RunInstances"                      (Can create resources)
🔴 "Action": "lambda:CreateFunction"                 (Can run code)
```

### Dangerous Resources:
```
🔴 "Resource": "*"                                   (All resources!)
🔴 "Resource": "arn:aws:s3:::*"                      (All S3 buckets)
🔴 "Resource": "arn:aws:kms:*:*:key/*"               (All KMS keys)
🔴 "Resource": "arn:aws:secretsmanager:*:*:secret:*" (All secrets)
```

---

# 🎯 ENUMERATION ATTACK PATH:

```
1. list-users ──► Get all usernames
      │
      ▼
2. list-attached-user-policies ──► Get policy ARNs
      │                              │
      ▼                              ▼
3. list-user-policies        get-policy ──► Get version ID
      │                              │
      ▼                              ▼
4. get-user-policy           get-policy-version ──► 🔴 SEE PERMISSIONS!
      │                              │
      └──────────────┬───────────────┘
                     ▼
              🔴 FOUND EXPLOITABLE PERMISSIONS!
                    (iam:PutUserPolicy, etc.)
                     ▼
              🔴 PRIVILEGE ESCALATION!
```

---

**This is the COMPLETE IAM Enumeration guide with LIVE examples!** 🚀
