# Accessing_Aws_Instance_by_cmd_promt
This repo shows you, How to access the AWS instances by the help of cmd promt.


# Accessing an AWS EC2 Instance: Two Methods

This comprehensive tutorial demonstrates **two reliable ways** to connect to your AWS EC2 instance, giving you secure SSH terminal access from your local machine.

---

## Overview

Whether you're a beginner or experienced user, this guide covers the most common methods for establishing SSH connections to your EC2 instances:

- **Command Prompt (Windows)** - Using native Windows tools
- **PuTTY SSH Client** - Using the popular third-party SSH client

Both methods provide secure, encrypted terminal access to your remote Linux instances.

---

## Prerequisites

Before you begin, ensure you have:

- ✅ **Active AWS EC2 instance** running Linux
- ✅ **SSH key pair** (`.pem` or `.ppk` file) associated with your instance
- ✅ **Security group** configured to allow SSH access (port 22)
- ✅ **Public IP address** or DNS name of your instance

> **Security Note:** Never expose your private key files publicly or share them with unauthorized users.

---

## Method 1: Connecting Using Command Prompt (Windows)

### Step-by-Step Instructions

#### 1. Get Connection Details from AWS Console

1. Navigate to your **EC2 Dashboard** in the AWS Console
2. Select your running instance
3. Click the **Connect** button
4. Choose the **SSH Client** tab
5. **Copy the example SSH command** provided by AWS

#### 2. Prepare Your Local Environment

1. Open **Command Prompt** (`cmd`) on your Windows machine
2. Navigate to the folder containing your `.pem` key file:
   ```bash
   cd C:\path\to\your\key\folder
   # Example: cd C:\Users\YourName\Downloads
   ```

#### 3. Set Proper Key Permissions (Important!)

```bash
# Ensure your .pem file has correct permissions
icacls "your-key.pem" /inheritance:r
icacls "your-key.pem" /grant:r "%username%":R
```

#### 4. Connect to Your Instance

Paste and execute the SSH command from AWS Console:

```bash
ssh -i "your-key.pem" ec2-user@your-instance-public-dns
```

**Example:**
```bash
ssh -i "my-key.pem" ec2-user@ec2-123-45-67-89.compute-1.amazonaws.com
```

#### 5. Accept the Connection

- When prompted about authenticity, type `yes` and press Enter
- **Success!** You should now see your EC2 instance terminal

---

## Method 2: Connecting Using PuTTY

### Step-by-Step Instructions

#### 1. Download and Install PuTTY

If you don't have PuTTY installed:
- Download from [official PuTTY website](https://www.putty.org/)
- Install both **PuTTY** and **PuTTYgen**

#### 2. Convert PEM to PPK (If Needed)

If you have a `.pem` file, convert it to `.ppk` format:

1. Open **PuTTYgen**
2. Click **Load** and select your `.pem` file
3. Click **Save private key** (choose **Yes** to skip passphrase)
4. Save as `your-key.ppk`

#### 3. Configure PuTTY Connection

1. **Open PuTTY**
2. In the **Host Name** field, enter your instance's:
   - Public IP address, OR
   - Public DNS name
   
   **Example:** `ec2-123-45-67-89.compute-1.amazonaws.com`

3. Ensure **Port** is set to `22` and **Connection type** is `SSH`

#### 4. Load Your Private Key

1. In the left panel, navigate to:  
   `Connection` → `SSH` → `Auth` → `Credentials`
2. Click **Browse** next to "Private key file for authentication"
3. Select your `.ppk` key file

#### 5. Save Session (Optional but Recommended)

1. Return to the **Session** category
2. Enter a name in **Saved Sessions** (e.g., "My EC2 Server")
3. Click **Save** for future use

#### 6. Connect to Your Instance

1. Click **Open** to start the connection
2. If prompted with a security alert, click **Accept**
3. **Login as:** Enter `ec2-user` (or appropriate username for your AMI)
4. **Success!** You're now connected to your EC2 instance

---

## Common Linux AMI Usernames

| AMI Type | Default Username |
|----------|------------------|
| Amazon Linux 2 | `ec2-user` |
| Ubuntu | `ubuntu` |
| CentOS | `centos` |
| RHEL | `ec2-user` |
| SUSE | `ec2-user` |
| Debian | `admin` |

---

## Troubleshooting Common Issues

### Connection Refused or Timeout

**Possible causes:**
- Security group doesn't allow SSH (port 22)
- Instance is not running
- Wrong IP address or DNS name

**Solutions:**
- Check security group rules in EC2 console
- Verify instance is in "running" state
- Confirm you're using the correct public IP/DNS

### Permission Denied (Publickey)

**Possible causes:**
- Wrong private key file
- Incorrect username
- Key file permissions issues

**Solutions:**
- Verify you're using the correct key file
- Check AMI-specific username (see table above)
- Fix key file permissions (Windows/Linux)

### "Host key verification failed"

**Solution:**
Remove old host key and try again:
```bash
ssh-keygen -R your-instance-public-dns
```

---

## Security Best Practices

### ✅ DO:
- Keep private key files secure and backed up
- Use strong passwords for key file encryption
- Regularly update your SSH client software
- Monitor connection logs for suspicious activity
- Use IAM roles when possible instead of key pairs

### ❌ DON'T:
- Share private key files via email or unsecured channels
- Use the same key pair for multiple critical instances
- Leave SSH port 22 open to 0.0.0.0/0 for production servers
- Store private keys on shared computers

---

## Advanced Tips

### Create SSH Config File (Windows/Linux)

Create `~/.ssh/config` to simplify connections:

```bash
Host myserver
    HostName ec2-123-45-67-89.compute-1.amazonaws.com
    User ec2-user
    IdentityFile ~/.ssh/my-key.pem
    Port 22
```

Then connect simply with: `ssh myserver`

### Use SSH Agent for Key Management

```bash
# Start SSH agent
ssh-agent bash

# Add your key
ssh-add /path/to/your-key.pem

# Connect without specifying key
ssh ec2-user@your-instance-dns
```

---

## Next Steps

Once connected to your EC2 instance, you can:

- Install software packages
- Configure services
- Transfer files using `scp` or `rsync`
- Set up additional users and security measures
- Deploy applications

---

## Additional Resources

- [AWS EC2 User Guide](https://docs.aws.amazon.com/ec2/latest/userguide/)
- [SSH Best Practices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)
- [PuTTY Documentation](https://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)

---

## License

This documentation is provided under the [MIT License](LICENSE).

---

**You're now ready to securely access your AWS EC2 instances using either method. Choose the one that best fits your workflow and environment!**
