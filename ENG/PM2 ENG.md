````markdown
# PM2 Guide for Node.js Projects

PM2 is a powerful process manager for Node.js applications that helps you keep your apps running continuously, automatically restarts them in case of crashes, and offers many useful features for monitoring and managing processes. This guide provides a comprehensive overview of PM2—including installation, basic commands, usage examples, and deployment tips.

---

## 1. What is PM2?

PM2 (Process Manager 2) is a production-ready process manager for Node.js that allows you to:

- **Run and manage multiple applications:** Start, stop, restart, or delete processes with simple commands.
- **Ensure high availability:** Automatically restart your application if it crashes.
- **Cluster mode:** Easily run your app in cluster mode to utilize all available CPU cores.
- **Monitor logs and performance:** View real-time logs and key metrics for each process.
- **Zero-downtime reloads:** Reload your application without losing connections.
- **Automatic startup:** Configure your apps to launch automatically when the server boots.

---

## 2. Installing PM2

### 2.1. Prerequisites

Ensure that Node.js and npm are installed on your server. You can check their versions by running:

```bash
node -v
npm -v
```
````

### 2.2. Installing PM2 Globally

To install PM2 globally, run:

```bash
npm install -g pm2
```

Verify the installation with:

```bash
pm2 -v
```

---

## 3. Running Your Application with PM2

### 3.1. Starting an Application

Navigate to your project directory:

```bash
cd /path/to/your/project
```

Then, start your Node.js application with PM2:

```bash
pm2 start app.js --name my-app
```

- Replace `app.js` with the entry point of your application.
- The `--name` flag assigns a custom name to the process (e.g., `my-app`).

### 3.2. Using the Watch Mode (Optional)

For development, you can enable watch mode so that PM2 restarts your application automatically when file changes are detected:

```bash
pm2 start app.js --name my-app --watch
```

---

## 4. Basic PM2 Commands and Examples

| Command                          | Description                                                                                                |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| `pm2 start app.js --name my-app` | Start your application with a specified name.                                                              |
| `pm2 list`                       | List all running processes managed by PM2.                                                                 |
| `pm2 stop my-app`                | Stop the application by name.                                                                              |
| `pm2 restart my-app`             | Restart the application (useful after code changes).                                                       |
| `pm2 delete my-app`              | Remove the application from PM2’s process list.                                                            |
| `pm2 logs my-app`                | View real-time logs for a specific application.                                                            |
| `pm2 flush`                      | Clear all log files.                                                                                       |
| `pm2 save`                       | Save the current process list to be resurrected later.                                                     |
| `pm2 resurrect`                  | Restore processes from the saved list.                                                                     |
| `pm2 startup`                    | Generate and configure a startup script so that PM2 and your processes start automatically on server boot. |

### Examples:

- **List Processes:**

  ```bash
  pm2 list
  ```

  This command displays the process ID, name, status, CPU and memory usage, and working directory for each process.

- **Stop and Restart:**

  ```bash
  pm2 stop my-app
  pm2 restart my-app
  ```

- **View Logs:**
  ```bash
  pm2 logs my-app
  ```

---

## 5. Ensuring Application Persistence

### 5.1. Saving the Process List

After you have started your application(s), save the current state:

```bash
pm2 save
```

This creates a dump file (usually named `dump.pm2`) that PM2 uses to resurrect your processes after a server reboot.

### 5.2. Configuring PM2 for Startup

Run the following command to generate a startup script:

```bash
pm2 startup
```

PM2 will output a command that you need to run with superuser privileges. For example:

```bash
sudo env PATH=$PATH:/usr/bin pm2 startup systemd -u your_user --hp /home/your_user
```

After executing the given command, run:

```bash
pm2 save
```

This ensures your applications are automatically restarted when the server boots.

---

## 6. Using an Ecosystem Configuration File

An ecosystem configuration file (`ecosystem.config.js`) allows you to define settings for multiple applications, environment variables, clustering, and deployment options.

### 6.1. Example `ecosystem.config.js`

```js
module.exports = {
  apps: [
    {
      name: "my-app",
      script: "app.js",
      exec_mode: "cluster", // Use cluster mode for load balancing
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

To start your application using the configuration file:

```bash
pm2 start ecosystem.config.js
```

---

## 7. PM2 Deployment

PM2 offers a built-in deployment system that automates updating your application on remote servers.

### 7.1. Setting Up Deployment

In your `ecosystem.config.js` file, configure the `deploy` section with:

- **user:** Your SSH username.
- **host:** Your server's IP address.
- **repo:** The SSH URL of your Git repository.
- **path:** The target path on your server.
- **post-deploy:** Commands to run after deployment (e.g., `npm install` and `pm2 reload`).

### 7.2. Deploying with PM2

1. **Set up deployment on the server:**
   ```bash
   pm2 deploy ecosystem.config.js production setup
   ```
2. **Deploy your latest changes:**
   ```bash
   pm2 deploy ecosystem.config.js production
   ```

---

## Conclusion

PM2 is an essential tool for Node.js production environments. It simplifies process management, ensures high availability through automatic restarts, and offers advanced features like clustering, log management, and zero-downtime reloads.

By using PM2, you can:

- **Start, stop, restart, and monitor** your Node.js applications with ease.
- **Automatically recover** from application crashes.
- **Set up auto-start** on server reboots.
- **Deploy** your applications quickly and efficiently.

For more details and advanced configurations, refer to the [official PM2 documentation](https://pm2.keymetrics.io/).

God save the JS!

```

```
