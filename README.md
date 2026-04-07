# Building a High-Efficiency Linux Home Lab on Windows

**Subtitle:** How I turned my Infinix X1 into a Web Server using WSL 2 and Nginx

---

## 1. The Goal (The "Why")

I wanted to learn Linux and Web Server management without the complexity of cloud providers (like [Oracle Cloud](https://www.oracle.com/cloud/) or [AWS](https://aws.amazon.com/)) or the heavy RAM usage of traditional Virtual Machines. My goal was to host a local website on my own hardware while gaining hands-on experience with Linux system administration.

---

## 2. My Setup (The Hardware)

- **Device:** Infinix X1 Laptop
- **Processor:** Intel Core i3 (10th Generation)
- **Memory:** 8GB RAM
- **Operating System:** Windows 10/11 with [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/) (Ubuntu)

---

## 3. The Implementation (The "How")

I chose [WSL 2 (Windows Subsystem for Linux)](https://docs.microsoft.com/en-us/windows/wsl/about) because it provides a real Linux kernel while sharing system resources efficiently—perfect for low-overhead development and server hosting.

### Key Steps Taken:

#### Hardware Optimization
- Enabled **Virtualization Technology (VT-x)** in the BIOS
- Enabled Windows features: Virtual Machine Platform and Windows Subsystem for Linux
- This allows the i3 processor to efficiently run two operating systems simultaneously

#### Environment Setup
1. Installed Ubuntu via PowerShell:
   ```powershell
   wsl --install
   ```
2. Verified installation:
   ```bash
   wsl -l -v
   ```

#### Server Deployment
1. Updated the package manager:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Installed [Nginx Web Server](https://nginx.org/):
   ```bash
   sudo apt install nginx -y
   ```
3. Started the service:
   ```bash
   sudo service nginx start
   ```
4. Verified the server status:
   ```bash
   sudo service nginx status
   ```

#### Customization
- Modified the default HTML index file (`/var/www/html/index.html`) to display a custom personalized message
- Configured Nginx to serve the custom website

---

## 4. Challenges & Solutions

### Challenge
Encountered a **"Catastrophic Failure"** during the initial WSL installation, preventing Ubuntu from launching.

### Solution
1. Manually enabled the **Virtual Machine Platform** feature in Windows:
   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
2. Set the hypervisor launch type to `auto` via BCDedit:
   ```powershell
   bcdedit /set hypervisorlaunchtype auto
   ```
3. Restarted the system to apply changes
4. This cleared the communication block between the CPU virtualization and Windows Hypervisor

---

## 5. Results

I now have a fully functional [Nginx](https://nginx.org/) web server accessible at `http://localhost`.

### Performance Metrics:
- **Startup Time:** ~2 seconds
- **RAM Usage:** Less than 1GB (compared to 2-4GB for traditional VMs)
- **Response Time:** Near-instant for local requests
- **Professional terminal:** Full Ubuntu environment with access to package managers and Linux tools

---

## 6. What I Learned

- How to configure and troubleshoot WSL 2 on Windows
- Linux system administration basics (package management, service control)
- Web server configuration and deployment with Nginx
- BIOS-level virtualization settings
- Windows Hypervisor architecture and troubleshooting

---

## 7. Next Steps

- [ ] Configure custom domain mapping with hosts file
- [ ] Set up SSL/TLS certificates for HTTPS
- [ ] Deploy a dynamic web application (Node.js, Python Flask, or PHP)
- [ ] Implement reverse proxy configuration
- [ ] Explore Docker containerization on WSL 2

---

## Useful Resources

- [WSL 2 Documentation](https://docs.microsoft.com/en-us/windows/wsl/)
- [Nginx Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)
- [Ubuntu Server Guide](https://ubuntu.com/server/docs)
- [DigitalOcean Nginx Tutorials](https://www.digitalocean.com/community/tags/nginx)

---

**Author:** Jitendra
**Project Date:** 2026
**Status:** Active and Running
