---
title: Docker Installation Lab - Container Runtime Setup
tags:
  - docker
  - containers
  - ubuntu
  - virtualization
  - beginner
draft: false
last_verified: 2025-09-07
lab_time: "15-30 minutes"
---

# Lab: Docker Installation on Ubuntu/Debian

**Objective**: Install and configure Docker Engine for container-based development and deployment

**Difficulty**: Beginner

**Time to Complete**: 15-30 minutes

> [!INFO]
> **Theory Path**: This lab supports the [Container Fundamentals Path](https://itlearn.claytownsend.com.au/containers) - complete that first for foundational knowledge.

## Prerequisites

### Knowledge Requirements
- Basic Linux command line navigation
- Understanding of package management (apt)
- Familiarity with sudo/root permissions

### Hardware/Software Requirements
- **Hardware**: 2GB RAM minimum, 10GB free disk space
- **Software**: Ubuntu 18.04+, Debian 10+, or compatible distribution
- **Network**: Internet access for package downloads
- **Lab Environment**: Physical machine, VM, or LXC container with nested virtualization

## Learning Outcomes

After completing this lab, you will be able to:
1. Install Docker Engine using the official APT repository
2. Configure Docker to run without sudo (optional)
3. Validate Docker installation with test containers
4. Understand Docker security considerations for homelab use

## Lab Steps

### Step 1: System Preparation

Update your package list and install prerequisites for Docker's APT repository.

```bash
# Update package database
sudo apt update

# Install packages for HTTPS repository access
sudo apt install -y ca-certificates curl
```

**Expected Output:**
```
Reading package lists... Done
Building dependency tree... Done
ca-certificates is already the newest version
curl is already the newest version
```

**Troubleshooting:**
- Network error: Check internet connectivity and DNS resolution
- Permission denied: Ensure you have sudo access

### Step 2: Add Docker's Official GPG Key

Add Docker's GPG key to verify package authenticity.

```bash
# Create keyrings directory
sudo install -m 0755 -d /etc/apt/keyrings

# Download and add Docker's GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

**Expected Output:**
```
(No output expected - commands complete silently on success)
```

### Step 3: Add Docker APT Repository

Configure the Docker repository for your system architecture.

```bash
# Add Docker repository to APT sources
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package database with new repository
sudo apt update
```

**Expected Output:**
```
Hit:1 https://download.docker.com/linux/ubuntu jammy InRelease
Reading package lists... Done
```

### Step 4: Install Docker Engine

Install Docker Engine, CLI, and related components.

```bash
# Install Docker packages
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Expected Output:**
```
The following NEW packages will be installed:
  containerd.io docker-buildx-plugin docker-ce docker-ce-cli docker-compose-plugin
Setting up docker-ce ...
```

### Step 5: Start and Enable Docker Service

Ensure Docker starts automatically on boot.

```bash
# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker
```

**Expected Output:**
```
Synchronizing state of docker.service with SysV service script
Executing: /lib/systemd/systemd-sysv-install enable docker
```

### Step 6: Optional - Add User to Docker Group

Allow running Docker without sudo (security consideration applies).

```bash
# Add current user to docker group
sudo usermod -aG docker $USER

# Apply group changes (logout/login or use newgrp)
newgrp docker
```

> [!WARNING]
> **Security Note**: Adding users to the docker group grants root-equivalent privileges. Only do this in trusted lab environments.

## Validation

Verify your Docker installation with these tests:

### Test 1: Docker Service Status
```bash
sudo systemctl status docker
```
**Expected Result**: Active (running) status with recent startup time

### Test 2: Docker Version Check
```bash
docker --version
docker compose version
```
**Expected Result**: 
```
Docker version 24.0.x, build xxxxx
Docker Compose version v2.20.x
```

### Test 3: Container Test
```bash
docker run hello-world
```
**Expected Result**: 
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

### Test 4: Docker Info
```bash
docker info
```
**Expected Result**: Detailed system information without errors

## Next Steps

- **Related Labs**: 
  - [Docker Compose Deployment Lab](/labs/virtualization/docker-compose)
  - [LXC Container Management Lab](/labs/virtualization/lxc-containers)
- **Advanced Configuration**: 
  - Docker daemon configuration (`/etc/docker/daemon.json`)
  - Custom bridge networks and storage drivers
- **Production Considerations**: 
  - Enable Docker Content Trust
  - Configure logging drivers
  - Set up monitoring with cAdvisor

## Common Issues

### Issue: "Cannot connect to the Docker daemon socket"
**Cause**: Docker service not running or user not in docker group
**Solution**: 
```bash
sudo systemctl start docker
# If using docker group:
newgrp docker
```

### Issue: "Package docker-ce has no installation candidate"
**Cause**: Docker repository not properly configured
**Solution**: Verify repository addition and run `sudo apt update`

### Issue: Permission denied while trying to connect to Docker daemon
**Cause**: Running docker commands without sudo and not in docker group
**Solution**: Either use `sudo docker` or add user to docker group (security implications)

## Cleanup (Optional)

To completely remove Docker:

```bash
# Stop Docker service
sudo systemctl stop docker

# Remove Docker packages
sudo apt purge -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Remove Docker data (WARNING: This deletes all containers and images)
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

# Remove repository configuration
sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.asc
```

---

## Lab Resources

- **Official Documentation**: [Docker Engine Installation Guide](https://docs.docker.com/engine/install/ubuntu/)
- **ITLearn Theory Path**: [Container Fundamentals](https://itlearn.claytownsend.com.au/containers)
- **Additional Reading**: 
  - [Docker Security Best Practices](https://docs.docker.com/engine/security/)
  - [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)

## Community

> [!HELP]
> **Need Help?**
> 
> Stuck on this lab? Join the [TWN Commons Discord](https://discord.gg/kgaMm6WJya) to:
> - Share screenshots of error messages
> - Get troubleshooting assistance for your specific OS
> - Discuss Docker security considerations
> - Help others with similar installation issues

---

*Last verified on 2025-09-07 with Ubuntu 22.04 LTS*