# Building a Professional Home Lab Web Server

**Subtitle:** Running a High-Performance Web Server on Windows Server VM

---

## 1. The Goal (The "Why")

I wanted to learn Web Server management and Windows Server administration without the complexity of cloud providers (like [Azure](https://azure.microsoft.com/) or [AWS](https://aws.amazon.com/)). My goal was to host a local website on my own hardware while gaining hands-on experience with web hosting, system administration, and network configuration.

---

## 2. My Setup (The Hardware)

- **Device:** Infinix X1 Laptop
- **Processor:** Intel Core i3 (10th Generation)
- **Memory:** 8GB RAM
- **Host OS:** Windows Server
- **Virtualization:** Hyper-V (Native) or VirtualBox
- **VM OS:** Windows Server 2019/2022
- **Web Server:** IIS (Internet Information Services)

---

## 3. The Implementation (The "How")

I chose **Windows Server** as the virtualization platform with **IIS** for professional web hosting capabilities. This allows complete control over web applications, SSL certificates, and enterprise-grade features.

### Key Steps Taken:

#### Host Machine Preparation
1. Enable **Virtualization Technology (VT-x)** in BIOS:
   - Restart computer and enter BIOS/UEFI
   - Find CPU virtualization settings (varies by motherboard)
   - Enable VT-x or AMD-V
   - Save and exit

2. Install Hyper-V or VirtualBox

#### Windows Server VM Setup
1. Create a new virtual machine:
   - Name: `HomeLabServer`
   - OS: Windows Server 2019/2022
   - Memory: 2GB RAM (minimum, 4GB recommended)
   - Storage: 40GB dynamic allocation
   - Generation: 2 (for Hyper-V)

2. Network Configuration:
   - **Adapter 1:** External Network (for internet access)
   - Assign static IP address for stability

3. Install Windows Server:
   - Download [Windows Server ISO](https://www.microsoft.com/en-us/windows-server)
   - Mount ISO and complete installation
   - Join Domain (optional)

#### IIS Installation on Windows Server
1. Open Server Manager:
   ```powershell
   # Run as Administrator
   Add-WindowsFeature -Name Web-Server -IncludeAllSubFeature
   ```

2. Or use GUI:
   - Server Manager → Add Roles and Features
   - Select Web Server (IIS)
   - Include: .NET Framework, ASP.NET, URL Rewrite

3. Verify IIS is running:
   ```powershell
   Get-Service W3SVC
   ```

4. Start IIS service:
   ```powershell
   Start-Service W3SVC
   ```

#### Website Deployment
1. Copy website files to IIS default directory:
   ```powershell
   Copy-Item "C:\path\to\index.html" -Destination "C:\inetpub\wwwroot\" -Force
   ```

2. Access IIS Manager:
   - Press `Windows Key + R`
   - Type `inetmgr`
   - Configure site properties

3. Set proper permissions:
   ```powershell
   # Grant IIS_IUSRS read permissions
   icacls "C:\inetpub\wwwroot" /grant "IIS_IUSRS:(OI)(CI)R"
   ```

4. Restart IIS:
   ```powershell
   iisreset
   ```

---

## 4. Networking & Access

### Finding Your VM's IP Address
```powershell
ipconfig
```
Look for IPv4 Address (typically 192.168.x.x or 10.x.x.x)

### Accessing the Website
- **From Host Machine:** Open a browser and navigate to:
  - `http://<VM-IP-ADDRESS>` (e.g., `http://192.168.1.50`)
  - `http://localhost` (if on the VM)

- **From Network:** `http://<VM-IP-ADDRESS>`

### Firewall Configuration
```powershell
# Allow HTTP traffic
netsh advfirewall firewall add rule name="HTTP" dir=in action=allow protocol=tcp localport=80

# Allow HTTPS traffic
netsh advfirewall firewall add rule name="HTTPS" dir=in action=allow protocol=tcp localport=443

# Allow Remote Desktop
netsh advfirewall firewall add rule name="RDP" dir=in action=allow protocol=tcp localport=3389
```

---

## 5. Troubleshooting Common Issues

### Issue: Cannot Access Website from Host
**Solution:**
1. Check IIS is running:
   ```powershell
   Get-Service W3SVC
   ```

2. Check VM IP address:
   ```powershell
   ipconfig
   ```

3. Verify firewall allows port 80:
   ```powershell
   netsh advfirewall firewall show rule name="HTTP"
   ```

4. Check IIS Default Site binding:
   - Open IIS Manager → Sites → Default Web Site
   - Edit Bindings → Verify HTTP on port 80

### Issue: RDP Connection Issues
**Solution:**
1. Enable Remote Desktop:
   ```powershell
   Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name fDenyTSConnections -Value 0
   ```

2. Verify firewall rule for RDP
3. Restart Remote Desktop Service:
   ```powershell
   Restart-Service TermService
   ```

### Issue: Slow Performance
**Solution:**
1. Increase VM allocated resources in Hyper-V/VirtualBox
2. Disable unnecessary Windows services
3. Enable Dynamic Memory (Hyper-V):
   ```powershell
   Set-VMMemory -VMName "HomeLabServer" -DynamicMemoryEnabled $true
   ```

---

## 6. Results

I now have a fully functional **IIS Web Server** running on Windows Server with professional hosting capabilities.

### Performance Metrics:
- **Startup Time:** ~20-30 seconds
- **RAM Usage:** 1.5GB - 3GB (configurable)
- **Response Time:** Near-instant for local requests
- **Network Access:** Full Windows Server features and management
- **Enterprise Features:** Certificate binding, URL Rewrite, Application Pools

---

## 7. What I Learned

- How to set up and configure Windows Server VMs
- Internet Information Services (IIS) administration
- Windows Firewall and Network configuration
- PowerShell scripting for server management
- BIOS-level virtualization settings
- Windows Server security best practices

---

## 8. Next Steps

- [ ] Configure SSL/TLS certificates for HTTPS
- [ ] Set up custom domain with DNS
- [ ] Deploy ASP.NET applications
- [ ] Implement URL Rewrite rules
- [ ] Set up automated backups
- [ ] Configure Application Pools
- [ ] Enable compression and caching
- [ ] Set up Windows Server Backup

---

## Windows Server Quick Reference

### IIS Management Commands
```powershell
# Start IIS
Start-Service W3SVC

# Stop IIS
Stop-Service W3SVC

# Restart IIS
Restart-Service W3SVC

# Reset IIS
iisreset

# Check IIS status
Get-Service W3SVC
```

### Network Commands
```powershell
# View IP configuration
ipconfig

# View firewall rules
netsh advfirewall firewall show rule all

# Disable firewall (not recommended)
netsh advfirewall set allprofiles state off
```

### File Management
```powershell
# Navigate to IIS root
cd C:\inetpub\wwwroot

# List files
dir

# Copy files
Copy-Item "source.html" -Destination "C:\inetpub\wwwroot\"
```

---

## Useful Resources

- [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
- [IIS Documentation](https://docs.microsoft.com/en-us/iis/)
- [Azure Virtual Machines](https://azure.microsoft.com/en-us/services/virtual-machines/)
- [Windows Server 2022 Guide](https://docs.microsoft.com/en-us/windows-server/get-started/windows-server-release-info)
- [Hyper-V Documentation](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)

---

**Author:** Jitendra
**Project Date:** 2026
**Platform:** Windows Server VM
**Status:** Active and Running
