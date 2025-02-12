# Guide to Server Setup and Management for Node.js Projects

This guide covers how to configure and manage a Linux server for deploying Node.js projects. You will learn how to update your system, use basic Linux commands, manage users and permissions, configure a firewall, install required components, monitor system resources, view logs, and automate deployments.

---

## Table of Contents

- [1. Server Setup (Linux)](#1-server-setup-linux)
  - [1.1. Updating the System](#11-updating-the-system)
  - [1.2. Basic Linux Commands](#12-basic-linux-commands)
  - [1.3. User and Permission Management](#13-user-and-permission-management)
  - [1.4. Configuring the Firewall (UFW)](#14-configuring-the-firewall-ufw)
- [2. Installing Required Components](#2-installing-required-components)
  - [2.1. Installing Node.js and npm](#21-installing-nodejs-and-npm)
  - [2.2. Installing Git](#22-installing-git)
  - [2.3. Installing PM2](#23-installing-pm2)
- [3. Monitoring and Logging](#3-monitoring-and-logging)
  - [3.1. Monitoring System Resources](#31-monitoring-system-resources)
  - [3.2. Viewing Application Logs](#32-viewing-application-logs)
- [4. Deployment Automation](#4-deployment-automation)
  - [4.1. Using Git Hooks (post-receive)](#41-using-git-hooks-post-receive)
  - [4.2. Setting up CI/CD with GitHub Actions](#42-setting-up-cicd-with-github-actions)
- [Conclusion](#conclusion)

---

## 1. Server Setup (Linux)

### 1.1. Updating the System

Before starting any work, update your system packages:

```bash
sudo apt update
sudo apt upgrade -y
```

### 1.2. Basic Linux Commands

Familiarize yourself with some basic commands that will help you navigate and manage your server:

- **Navigation:**

  - `cd` – Change directory  
    _Example:_ `cd /var/www`
  - `ls` – List files and directories  
    _Example:_ `ls -la`

- **File and Directory Operations:**

  - `cp` – Copy files or directories  
    _Example:_ `cp file.txt /path/to/destination/`
  - `mv` – Move or rename files  
    _Example:_ `mv oldname.txt newname.txt`
  - `rm` – Remove files or directories  
    _Example:_ `rm file.txt` or `rm -r folder/`
  - `mkdir` – Create a new directory  
    _Example:_ `mkdir new_folder`

- **Managing Permissions:**
  - `chmod` – Change file or directory permissions  
    _Example:_ `chmod 755 script.sh`
  - `chown` – Change file or directory ownership  
    _Example:_ `chown user:group file.txt`

### 1.3. User and Permission Management

Key commands for managing users and permissions:

- **Using sudo:**  
  Run commands with superuser privileges:
  ```bash
  sudo apt update
  ```
- **Adding a User:**  
  Create a new user:
  ```bash
  sudo adduser username
  ```
- **Modifying a User:**  
  Add a user to a group (e.g., `sudo` group):
  ```bash
  sudo usermod -aG sudo username
  ```

### 1.4. Configuring the Firewall (UFW)

UFW (Uncomplicated Firewall) helps protect your server by controlling incoming and outgoing traffic.

1. **Install UFW (if not already installed):**
   ```bash
   sudo apt install ufw -y
   ```
2. **Allow SSH connections:**
   ```bash
   sudo ufw allow ssh
   ```
3. **Allow common web ports (HTTP/HTTPS):**
   ```bash
   sudo ufw allow 80/tcp
   sudo ufw allow 443/tcp
   ```
4. **Enable UFW:**
   ```bash
   sudo ufw enable
   ```
5. **Check UFW status:**
   ```bash
   sudo ufw status
   ```

---

## 2. Installing Required Components

### 2.1. Installing Node.js and npm

Install Node.js (LTS version recommended) and npm:

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Verify installation:

```bash
node -v
npm -v
```

### 2.2. Installing Git

Git is essential for version control and collaboration:

```bash
sudo apt install git -y
```

Verify Git installation:

```bash
git --version
```

### 2.3. Installing PM2

PM2 is a process manager for Node.js applications. It keeps your apps running, automatically restarts them if they crash, and provides useful monitoring and management tools.

Install PM2 globally:

```bash
npm install -g pm2
```

Verify installation:

```bash
pm2 -v
```

---

## 3. Monitoring and Logging

### 3.1. Monitoring System Resources

To monitor your server’s performance, you can use the following commands:

- **top:** Displays a real-time list of running processes.
- **htop:** An enhanced version of top (install with `sudo apt install htop` if needed).
- **free -h:** Shows memory usage.
- **df -h:** Displays disk space usage.

### 3.2. Viewing Application Logs

If you run your Node.js application with PM2, you can view its logs using:

```bash
pm2 logs my-app
```

To clear all logs:

```bash
pm2 flush
```

---

## 4. Deployment Automation

### 4.1. Using Git Hooks (post-receive)

Git hooks allow you to automatically update your project on the server when new commits are pushed.

1. **Navigate to the hooks directory of your repository on the server:**
   ```bash
   cd /path/to/your/project/.git/hooks
   ```
2. **Create or edit the `post-receive` file:**
   ```bash
   sudo nano post-receive
   ```
3. **Insert the following script:**
   ```bash
   #!/bin/bash
   # Set the working directory and update the repository
   GIT_WORK_TREE=/path/to/your/project git pull origin main
   cd /path/to/your/project
   npm install
   pm2 restart my-app
   echo "✅ Automatic deployment completed!"
   ```
4. **Make the script executable:**
   ```bash
   sudo chmod +x post-receive
   ```

### 4.2. Setting up CI/CD with GitHub Actions

Automate deployments with GitHub Actions so that when you push to the `main` branch, your server updates automatically.

1. **Create a directory and workflow file in your project:**

   - Directory: `.github/workflows/`
   - File: `deploy.yml`

2. **Example `deploy.yml`:**

   ```yaml
   name: Deploy Node.js App

   on:
     push:
       branches:
         - main

   jobs:
     deploy:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout repository
           uses: actions/checkout@v2

         - name: Deploy to Server via SSH
           uses: appleboy/ssh-action@v0.1.5
           with:
             host: ${{ secrets.SERVER_IP }}
             username: ${{ secrets.SERVER_USER }}
             key: ${{ secrets.SSH_PRIVATE_KEY }}
             script: |
               cd /path/to/your/project
               git pull origin main
               npm install
               pm2 restart my-app
   ```

3. **Add the following secrets to your GitHub repository:**
   - `SERVER_IP` – Your server's IP address.
   - `SERVER_USER` – The SSH username (e.g., `root`).
   - `SSH_PRIVATE_KEY` – The private SSH key for accessing your server.

---

God Save The JS!