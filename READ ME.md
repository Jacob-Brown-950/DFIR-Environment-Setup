# Windows 10-11 Malware Analysis Lab Setup

## Overview
This project guides you through setting up a Windows 10 VM, configuring Flare-VM for malware analysis, and creating a secure environment using pfSense and Remnux to contain and analyze malware safely. Here is a diagram to visualize how the traffic will be isolated.

![Image](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/Malware%20Analysis%20Network%20Diagram.PNG)

## Prerequisites
- **Oracle VirtualBox**: Download [here](https://www.virtualbox.org/).
- **Windows 10 ISO File**: [here](https://www.bing.com/ck/a?!&&p=ab5b918e3ff54241JmltdHM9MTcyOTQ2ODgwMCZpZ3VpZD0yOWEzMGNjMC00YzViLTZjZjctMDhkYi0xODRkNGQ3MjZkZjAmaW5zaWQ9NTIxMA&ptn=3&ver=2&hsh=3&fclid=29a30cc0-4c5b-6cf7-08db-184d4d726df0&psq=windows+10+iso&u=a1aHR0cHM6Ly93d3cubWljcm9zb2Z0LmNvbS9lbi11cy9zb2Z0d2FyZS1kb3dubG9hZC93aW5kb3dzMTBJU08_bXNvY2tpZD0yOWEzMGNjMDRjNWI2Y2Y3MDhkYjE4NGQ0ZDcyNmRmMA&ntb=1).
- **pfSense ISO**: Download [here](https://www.pfsense.org/download/). I will also include this in the repository.
- **Remnux OVA**: Get the OVA file from [Remnux Official Site](https://docs.remnux.org/install-distro/get-virtual-appliance).

---

## Steps

### 1. Setting up Windows 10 VM
- Download and install **Oracle VirtualBox**.
- Import the **Windows 10 ISO** file.
- Configure your Windows 10 VM with the following specs:
  - **RAM**: 2GB
  - **Storage**: 80GB
- Follow the Windows installation process:
  - Select "I don't have a product key" and choose "Windows 10 Pro".
  - Ensure your username has no spaces or special characters.

**Image**  
![Windows 10 VM Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/3%202gb%20Ram%20for%20Windows%2010%20VM.PNG)
![Windows 10 VM Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/4%2080Gb%20Storage%20for%20Windows%20VM.PNG)
![Windows 10 VM Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/5%20Download%20process.PNG)

---

### 2. Installing Flare-VM
- Spin up the Windows 10 VM and follow the **Pre-Installation** steps in the [Flare-VM repo](https://github.com/mandiant/flare-vm).
  - Remove Windows Security features via Group Policy and Windows Defender settings.
- Download and run the PowerShell installation script from the repo in your **Desktop** directory.

### 3. Configuring Flare-VM Features
- The Flare-VM installer will prompt you to choose additional features. The default set is sufficient, but feel free to add more.
- The installation may take up to an hour, depending on selected features.

**Image**  
![Flare-VM Feature Selection](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/6%20Install%20FlareVM%20and%20choose%20what%20features%20you%20want%2C%20i%20left%20mine%20default.PNG)

---

### 4. Setting Up pfSense
- Download the pfSense ISO from the repository.
- Configure the pfSense VM:
  - **RAM**: 1-2GB
  - **Storage**: 12GB (for potential plugins)
- Add a second network adapter:
  - Go to `File > Tools > Network Manager` in VirtualBox.
  - Ensure DHCP is disabled.

**Image**  
![pfSense Network Adapter Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/7%20Setup%20pfsense%20with%20baseline%20RAM.PNG)
![pfSense Network Adapter Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/8%20Give%2012%20gb%20of%20space%20to%20pfsense.PNG)
![pfSense Network Adapter Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/9%20Create%202nd%20network%20adapter%20in%20filestoolsnetworkmanmager.PNG)
![pfSense Network Adapter Setup](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/10%20Enable%20second%20adapter%20on%20pfsense%20and%20set%20to%20host%20only.PNG)
---

### 5. Installing pfSense
- Start the pfSense VM and follow the installation process (just press enter through defaults).
- After installation, reboot the VM and unmount the ISO.
- Boot up pfSense and verify the setup screen.

**Image**  
![pfSense Setup Screen](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/11%20Remove%20the%20ISO%20image%20after%20you%20install%20pfsense.PNG)
![pfSense Setup Screen](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/12%20Once%20pfsense%20is%20done%20booting%20you%20should%20see%20this%20screen.PNG)

---

### 6. Importing Remnux
- Download and import the Remnux OVA into VirtualBox.
- Set the network adapter for Remnux to the **host-only** adapter created earlier.

**Image**  
![Remnux Import and Network Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/13%20Remnux%20Installed.PNG)
![Remnux Import and Network Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/14%20change%20Remna%20network%20adapter%20settings%20to%20host%20only.PNG)

---

### 7. Configuring Flare-VM for Isolation
- Once Flare-VM installation is complete, switch its network adapter to the **host-only** adapter.
- Access the pfSense web GUI via the default gateway and configure the LAN interface and DNS settings.

**Image**  
![Flare-VM Connected to pfSense](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/15%20Navigate%20to%20pfsense%20GUI.PNG)
![Flare-VM Connected to pfSense](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/16%20Setting%20pfsense%20name%20and%20DNS%20settings.PNG)
![Flare-VM Connected to pfSense](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/17%20Set%20LAN%20IP%20for%20pfsense.PNG)
---

### 8. Configuring Firewall Rules in pfSense
- Go to `Firewall > Rules` and create rules to prevent the FlareVM from accessing your local network while maintaining internet connectivity.

**Image**  
![Firewall Rules in pfSense](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/18%20Setup%20your%20firewall%20rule%20to%20isolate%20it%20from%20local%20network%2C%20but%20still%20allow%20access.PNG)

---

### 9. Setting Up Remnux for Malware Analysis
- Boot up Remnux and set the network configuration to match the pfSense LAN IP.
- Start the **fakedns** service for simulating DNS traffic during analysis.

**Image**  
![Remnux DNS Configuration](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/21%20Configure%20Remnux%20netplan.PNG)

---

### 10. Downloading and Testing Malware
- Download a malware sample like **WannaCry** from [MalwareZoo](https://github.com/abuisa/MalwareZoo).
- Before running the malware, Turn off your PfSense, and activate Remnux fakedns

**Image**  
![Malware Execution in Flare-VM](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/24%20I%20got%20hacked!.PNG)

---

### 11. Restoring VM Snapshots
- Take snapshots of your VM before malware detonation for easy restoration.
- After testing, restore the snapshot to reset your environment.

**Image**  
![Snapshot Management in VirtualBox](https://github.com/Jacob-Brown-950/Malware-Analysis-Environment/blob/main/Screenshots/25%20Restore%20your%20infected%20machine%20to%20its%20original%20snapshot.PNG)

---

## Additional Notes
- Always ensure proper isolation when testing malware.
- Keep your snapshots up to date to minimize downtime when restoring VMs.

---

## Conclusion
This guide sets up a secure and isolated environment for malware analysis using Windows 10, Flare-VM, pfSense, and Remnux. More advanced labs and exercises will follow!
