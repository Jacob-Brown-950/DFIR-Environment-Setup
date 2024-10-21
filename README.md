# Windows 10-11 Malware Analysis Lab Setup

## Overview
This project guides you through setting up a Windows 10 VM, configuring Flare-VM for malware analysis, and creating a secure environment using pfSense and Remnux to safely contain and analyze malware. The diagram below illustrates how network traffic will be isolated.

![Network Diagram](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/Malware%20Analysis%20Network%20Diagram.PNG)

---

## Prerequisites
- **Oracle VirtualBox**: Download [here](https://www.virtualbox.org/).
- **Windows 10 ISO File**: [Download here](https://www.microsoft.com/en-us/software-download/windows10ISO).
- **pfSense ISO**: Download [here](https://www.pfsense.org/download/). This is also available in the repository.
- **Remnux OVA**: Get it from [Remnux Official Site](https://docs.remnux.org/install-distro/get-virtual-appliance).

---

## Steps

### 1. Setting up Windows 10 VM
- Download and install **Oracle VirtualBox**.
- Import the **Windows 10 ISO** file.
- Configure your Windows 10 VM:
  - **RAM**: 2GB
  - **Storage**: 80GB
- Follow the Windows installation:
  - Select "I don't have a product key" and choose "Windows 10 Pro".
  - Ensure your username has no spaces or special characters.

**Images**  
![Windows 10 VM RAM](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/3%202gb%20Ram%20for%20Windows%2010%20VM.PNG)  
![Windows 10 VM Storage](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/4%2080Gb%20Storage%20for%20Windows%20VM.PNG)  
![Windows 10 Installation](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/5%20Download%20process.PNG)

---

### 2. Installing Flare-VM
- Spin up the Windows 10 VM and follow the **Pre-Installation** steps in the [Flare-VM repo](https://github.com/mandiant/flare-vm).
  - Disable Windows Security features through Group Policy and Windows Defender.
- Download and run the PowerShell installation script from the repo in your **Desktop** directory.

### 3. Configuring Flare-VM Features
- Choose additional features as prompted; the default set is sufficient, but you can add more.
- The installation may take up to an hour.

**Image**  
![Flare-VM Feature Selection](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/6%20Install%20FlareVM%20and%20choose%20what%20features%20you%20want%2C%20i%20left%20mine%20default.PNG)

---

### 4. Setting Up pfSense
- Download the pfSense ISO from the repository.
- Configure the pfSense VM:
  - **RAM**: 1-2GB
  - **Storage**: 12GB
- Add a second network adapter:
  - Go to `File > Tools > Network Manager` in VirtualBox and disable DHCP.

**Images**  
![pfSense RAM Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/7%20Setup%20pfsense%20with%20baseline%20RAM.PNG)  
![pfSense Storage Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/8%20Give%2012%20gb%20of%20space%20to%20pfsense.PNG)  
![pfSense Network Adapter](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/9%20Create%202nd%20network%20adapter%20in%20filestoolsnetworkmanmager.PNG)  
![Enable Second Adapter](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/10%20Enable%20second%20adapter%20on%20pfsense%20and%20set%20to%20host%20only.PNG)

---

### 5. Installing pfSense
- Start the pfSense VM and follow the installation process, pressing enter through defaults.
- Reboot the VM and unmount the ISO.
- Verify the setup screen after booting.

**Images**  
![Unmount ISO](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/11%20Remove%20the%20ISO%20image%20after%20you%20install%20pfsense.PNG)  
![pfSense Setup Screen](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/12%20Once%20pfsense%20is%20done%20booting%20you%20should%20see%20this%20screen.PNG)

---

### 6. Importing Remnux
- Download and import the Remnux OVA into VirtualBox.
- Set the network adapter for Remnux to the **host-only** adapter.

**Images**  
![Remnux Installation](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/13%20Remnux%20Installed.PNG)  
![Remnux Network Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/14%20change%20Remna%20network%20adapter%20settings%20to%20host%20only.PNG)

---

### 7. Configuring Flare-VM for Isolation
- Switch Flare-VM’s network adapter to **host-only**.
- Access the pfSense web GUI via the default gateway to configure LAN and DNS settings.

**Images**  
![Access pfSense GUI](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/15%20Navigate%20to%20pfsense%20GUI.PNG)  
![Set pfSense DNS](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/16%20Setting%20pfsense%20name%20and%20DNS%20settings.PNG)  
![LAN Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/17%20Set%20LAN%20IP%20for%20pfsense.PNG)

---

### 8. Configuring Firewall Rules in pfSense
- Go to `Firewall > Rules` and create rules to isolate Flare-VM from the local network while allowing internet access.

**Image**  
![Firewall Rules](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/18%20Setup%20your%20firewall%20rule%20to%20isolate%20it%20from%20local%20network%2C%20but%20still%20allow%20access.PNG)

---

### 9. Setting Up Remnux for Malware Analysis
- Boot up Remnux and match the network configuration with the pfSense LAN IP.
- Activate the **fakedns** service to simulate DNS traffic.

**Image**  
![Remnux DNS Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/21%20Configure%20Remnux%20netplan.PNG)

---

### 10. Downloading and Testing Malware
- Download a malware sample like **WannaCry** from [MalwareZoo](https://github.com/abuisa/MalwareZoo).
- Before executing the malware, deactivate pfSense and start Remnux’s fakedns.

**Image**  
![Malware Execution](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/24%20I%20got%20hacked!.PNG)

---

### 11. Restoring VM Snapshots
- Take snapshots before malware detonation for easy restoration.
- Restore snapshots after testing to reset the environment.

**Image**  
![Snapshot Restoration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/25%20Restore%20your%20infected%20machine%20to%20its%20original%20snapshot.PNG)

---

## Additional Notes
- Ensure proper isolation when testing malware.
- Regularly update snapshots to minimize downtime when restoring VMs.

---

## Conclusion
This guide provides a secure and isolated environment for malware analysis using Windows 10, Flare-VM, pfSense, and Remnux. Stay tuned for more advanced labs and exercises!
