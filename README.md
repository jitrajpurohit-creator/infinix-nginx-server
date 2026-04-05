Project Title: Building a High-Efficiency Linux Home Lab on Windows
Subtitle: How I turned my Infinix X1 into a Web Server using WSL 2 and Nginx.

1. The Goal (The "Why")
I wanted to learn Linux and Web Server management without the complexity of cloud providers (like Oracle/AWS) or the heavy RAM usage of traditional Virtual Machines. My goal was to host a local website on my own hardware.

2. My Setup (The Hardware)
Device: Infinix X1 Laptop

Processor: Intel Core i3 (10th Generation)

Memory: 8GB RAM

Operating System: Windows 10/11 with WSL 2 (Ubuntu)

3. The Implementation (The "How")
I chose WSL 2 (Windows Subsystem for Linux) because it provides a real Linux kernel while sharing system resources efficiently.

Key Steps Taken:

Hardware Optimization: Enabled Virtualization in the BIOS and Windows features to allow the i3 processor to run two operating systems at once.

Environment Setup: Installed Ubuntu via PowerShell using wsl --install.

Server Deployment: * Updated the package manager: sudo apt update

Installed Nginx Web Server: sudo apt install nginx -y

Started the service: sudo service nginx start

Customization: Modified the default HTML index file to display a custom personalized message.

4. Challenges & Solutions
Challenge: Encountered a "Catastrophic Failure" during the initial WSL installation.

Solution: Manually enabled the Virtual Machine Platform and set the hypervisorlaunchtype to auto via the Windows BCDedit tool. This cleared the communication block between the CPU and Windows.

5. Results
I now have a fully functional Nginx web server accessible at http://localhost. The system is extremely fast, uses less than 1GB of RAM, and provides a professional-grade terminal for learning.
