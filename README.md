# Linux Server Administration and Deployment

## 1. Linux CLI and System Administration

Learn and practice the essential Linux command-line operations.

### Topics
- Navigate directories and manage files.
- Create, copy, move, rename, and delete files and directories.
- Understand file permissions and ownership.
- Create and manage users and groups.
- Monitor and manage processes.
- Manage system services with `systemctl`.
- Generate and use SSH keys for secure remote access.

### Practice Commands

```bash
# Navigate and inspect files
pwd
ls -lah
cd /path/to/directory

# Create and manage files
touch example.txt
mkdir example-dir
cp example.txt example-copy.txt
mv example-copy.txt example-renamed.txt
rm example-renamed.txt

# View and edit file permissions
ls -l
chmod 640 example.txt
sudo chown "$USER":"$USER" example.txt

# Manage users and groups
sudo adduser labuser
sudo groupadd labgroup
sudo usermod -aG labgroup labuser
id labuser
getent group labgroup

# Inspect and manage processes
ps aux
top
pgrep -a ssh
kill <PID>

# Manage services
systemctl status ssh
sudo systemctl restart ssh
sudo systemctl enable ssh

# Generate an SSH key
ssh-keygen -t ed25519 -C "linux-lab"
```

Replace placeholders such as `/path/to/directory` and `<PID>` with actual values. Review a command before using it, especially commands that delete files or terminate processes.

## 2. Networking and Security

Learn how a Linux server communicates with other systems and how access is controlled.

### Topics
- IPv4 addressing, subnet masks, and default gateways.
- DNS records and name resolution.
- Common network ports and the services that use them.
- Host-based firewall configuration.
- Cloud security groups and inbound/outbound rules.
- Secure SSH access and least-privilege access.

### Practice Commands

```bash
# View IP addresses and routes
ip address
ip route

# Test DNS resolution
getent hosts example.com
dig example.com

# Check connectivity
ping -c 4 1.1.1.1
curl -I https://example.com

# Inspect listening ports and processes
sudo ss -tulpn

# Check firewall status and rules (Ubuntu)
sudo ufw status verbose
```

If `dig` is unavailable, install it with:

```bash
sudo apt update
sudo apt install -y dnsutils
```

### Common Ports

| Port | Protocol | Typical Use |
|---:|---|---|
| 22 | TCP | SSH |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |

### Security Checklist

- Allow only the ports required by the service.
- Restrict SSH access to trusted IP addresses when possible.
- Use SSH keys and protect private keys.
- Avoid exposing databases and internal services publicly.
- Configure both the server firewall and cloud security group, when applicable.
- Keep the operating system and packages updated.

**Important:** Before changing SSH or firewall rules on a remote server, confirm that your current access will remain available. Keep an existing session open until you verify a new connection.

## 3. Deploy a Linux Server

Follow this workflow to create, secure, deploy, and test a Linux server.

### Step 1: Create a Virtual Machine

Create a VM using a platform such as VirtualBox, KVM/libvirt, or a cloud provider. Select a supported Linux distribution, such as Ubuntu Server, and note its IP address and login user.

### Step 2: Configure Linux

Connect to the VM console or log in over SSH, then update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

Check the system and network:

```bash
hostnamectl
ip address
ip route
df -h
free -h
```

### Step 3: Secure Access

Generate an SSH key on your client computer if you do not already have one:

```bash
ssh-keygen -t ed25519 -C "linux-lab"
```

For a VM with SSH password access enabled, copy the public key:

```bash
ssh-copy-id <username>@<server-ip>
```

Then test key-based access:

```bash
ssh <username>@<server-ip>
```

Replace `<username>` and `<server-ip>` with your actual login user and server IP. Do not share your private key. If using a cloud VM, configure its SSH key and security group according to the provider's instructions.

### Step 4: Deploy a Test Service

Install Nginx as a simple web service:

```bash
sudo apt install -y nginx
sudo systemctl enable --now nginx
sudo systemctl status nginx
```

Confirm that the service is listening:

```bash
sudo ss -tulpn | grep ':80'
```

### Step 5: Configure Network Access

For a local VM, configure the VM's network mode and host firewall as needed. For a cloud VM, add an inbound security-group rule for TCP port `80` from an appropriate source. Avoid opening ports that the lab does not need.

If using UFW on Ubuntu and you need to allow web traffic:

```bash
sudo ufw allow 80/tcp
sudo ufw status
```

Enable UFW only after allowing the access you need, especially SSH, so you do not lock yourself out. For example, if SSH is required:

```bash
sudo ufw allow OpenSSH
```

### Step 6: Test Connectivity

On the server, test the local service:

```bash
curl -I http://localhost
```

From another machine that can reach the VM, test using its address:

```bash
curl -I http://<server-ip>
```

You can also open `http://<server-ip>` in a browser. If the test fails, check the service status, listening ports, VM network configuration, DNS (if using a domain), host firewall, and cloud security-group rules.

## Expected Outcome

By completing this lab, you should be able to:

- Perform basic Linux CLI and system administration tasks.
- Manage permissions, users, groups, processes, and services.
- Connect securely to a server using SSH keys.
- Check IP addressing, DNS resolution, ports, and firewall rules.
- Deploy a basic web service and verify connectivity.

## Lab Notes

Record the following for your own environment:

- **Linux distribution/version:** 
- **VM or cloud platform:** 
- **Server IP address:** 
- **SSH connection tested:** Yes / No
- **Service deployed:** 
- **Connectivity test result:** 
- **Issues and fixes:** 

---
**Practice safely:** Use a test VM or lab server. Commands, firewall rules, and cloud security-group settings may differ depending on your environment.
