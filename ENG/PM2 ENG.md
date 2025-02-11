````markdown
# PM2 Guide for Node.js Projects

PM2 is a popular process manager for Node.js applications that helps you run, monitor, and automatically restart your applications in production environments. It provides features such as automatic restarts on crashes, load balancing in cluster mode, and convenient log management. This guide explains how to install PM2, start and manage your application, and introduces the most common commands with examples.

---

## Table of Contents

- [1. Installing PM2](#1-installing-pm2)
  - [1.1. Prerequisites](#11-prerequisites)
  - [1.2. Global Installation](#12-global-installation)
- [2. Running an Application with PM2](#2-running-an-application-with-pm2)
  - [2.1. Starting an Application](#21-starting-an-application)
  - [2.2. Watch Mode for Development](#22-watch-mode-for-development)
- [3. Basic PM2 Commands](#3-basic-pm2-commands)
  - [3.1. Listing Processes](#31-listing-processes)
  - [3.2. Stopping an Application](#32-stopping-an-application)
  - [3.3. Restarting an Application](#33-restarting-an-application)
  - [3.4. Deleting an Application](#34-deleting-an-application)
  - [3.5. Viewing Logs](#35-viewing-logs)
  - [3.6. Flushing Logs](#36-flushing-logs)
- [4. Managing Processes Automatically](#4-managing-processes-automatically)
  - [4.1. Saving Process List](#41-saving-process-list)
  - [4.2. Configuring Auto-Start on Server Boot](#42-configuring-auto-start-on-server-boot)
- [5. Exporting and Importing Processes](#5-exporting-and-importing-processes)
- [6. Using an Ecosystem Configuration File](#6-using-an-ecosystem-configuration-file)
- [7. PM2 Deploy (Automated Deployment)](#7-pm2-deploy-automated-deployment)
- [Conclusion](#conclusion)

---

## 1. Installing PM2

### 1.1. Prerequisites

Before installing PM2, ensure that Node.js and npm are installed on your server. Check the versions by running:

```bash
node -v
npm -v
```
````

### 1.2. Global Installation

Install PM2 globally using npm:

```bash
npm install -g pm2
```

Verify that PM2 is installed correctly:

```bash
pm2 -v
```

---

## 2. Running an Application with PM2

### 2.1. Starting an Application

Navigate to your project directory:

```bash
cd /path/to/your/project
```

Start your Node.js application using PM2 and assign it a custom name:

```bash
pm2 start app.js --name my-app
```

> **Note:** Replace `app.js` with your application's entry file and `my-app` with your desired process name.

### 2.2. Watch Mode for Development

For development purposes, you can enable watch mode so that PM2 restarts your application automatically when files change:

```bash
pm2 start app.js --name my-app --watch
```

---

## 3. Basic PM2 Commands

### 3.1. Listing Processes

To list all running processes managed by PM2:

```bash
pm2 list
```

This command displays a table with information such as process ID, name, status, CPU usage, memory usage, and the working directory.

### 3.2. Stopping an Application

Stop a specific application by name or by its process ID:

```bash
pm2 stop my-app
```

Or by ID:

```bash
pm2 stop 0
```

### 3.3. Restarting an Application

Restart your application (useful after making code changes):

```bash
pm2 restart my-app
```

Or by ID:

```bash
pm2 restart 0
```

### 3.4. Deleting an Application

To remove an application from PM2’s process list:

```bash
pm2 delete my-app
```

Or by ID:

```bash
pm2 delete 0
```

### 3.5. Viewing Logs

To view real-time logs for a specific application:

```bash
pm2 logs my-app
```

To see logs for all processes:

```bash
pm2 logs
```

### 3.6. Flushing Logs

Clear all log files managed by PM2 with:

```bash
pm2 flush
```

---

## 4. Managing Processes Automatically

### 4.1. Saving the Process List

After starting your applications, save the current state so that PM2 can resurrect them after a server reboot:

```bash
pm2 save
```

### 4.2. Configuring Auto-Start on Server Boot

Generate and configure a startup script to automatically start PM2 processes when the server boots:

```bash
pm2 startup
```

Follow the on-screen instructions (usually, you will need to run a command with `sudo`). After that, run:

```bash
pm2 save
```

---

## 5. Exporting and Importing Processes

### 5.1. Exporting the Process List

Save the current process list to a dump file:

```bash
pm2 dump
```

### 5.2. Restoring Processes

To restore processes from the saved dump (e.g., after a server restart):

```bash
pm2 resurrect
```

---

## 6. Using an Ecosystem Configuration File

An ecosystem configuration file (`ecosystem.config.js`) simplifies managing multiple applications, environment variables, and clustering.

### 6.1. Example: ecosystem.config.js

```js
module.exports = {
  apps: [
    {
      name: "my-app",
      script: "app.js",
      exec_mode: "cluster", // Run in cluster mode to utilize all CPU cores
      instances: "max", // Spawn as many instances as there are CPU cores
      watch: false, // Disable watch mode in production
      env: {
        NODE_ENV: "production",
        PORT: 3000,
      },
    },
  ],
  deploy: {
    production: {
      user: "your_server_user",
      host: "your_server_ip",
      ref: "origin/main",
      repo: "git@github.com:yourusername/yourrepo.git",
      path: "/path/to/your/project",
      "post-deploy":
        "npm install && pm2 reload ecosystem.config.js --env production",
    },
  },
};
```

### 6.2. Starting with Ecosystem File

To start your applications using the configuration file:

```bash
pm2 start ecosystem.config.js
```

---

## 7. PM2 Deploy (Automated Deployment)

PM2 provides a built-in deployment system that can automatically update your application on a remote server when you push new changes.

### 7.1. Setting Up Deployment in ecosystem.config.js

In the `deploy` section of your configuration file, define the following:

- **user:** SSH username on your server.
- **host:** Your server’s IP address.
- **repo:** The SSH URL of your repository.
- **path:** The target directory on the server.
- **post-deploy:** Commands to execute after deployment (e.g., `npm install` and `pm2 reload`).

### 7.2. Deploying Your Application

1. Set up the deployment:
   ```bash
   pm2 deploy ecosystem.config.js production setup
   ```
2. Deploy the latest version:
   ```bash
   pm2 deploy ecosystem.config.js production
   ```

---

God Save The JS!

```

```
