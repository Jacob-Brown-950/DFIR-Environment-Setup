# Windows 10-11 Malware Analysis Lab Setup

## Overview
This project guides you through setting up a Windows 10 VM, configuring Flare-VM for malware analysis, and creating a secure environment using pfSense and Remnux to contain and analyze malware safely.

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
![Windows 10 VM Setup](path/to/image1.png)

---

### 2. Installing Flare-VM
- Spin up the Windows 10 VM and follow the **Pre-Installation** steps in the [Flare-VM repo](https://github.com/mandiant/flare-vm).
  - Remove Windows Security features via Group Policy and Windows Defender settings.
- Download and run the PowerShell installation script from the repo in your **Desktop** directory.

**Image**  
![Flare-VM Installation](path/to/image2.png)

---

### 3. Configuring Flare-VM Features
- The Flare-VM installer will prompt you to choose additional features. The default set is sufficient, but feel free to add more.
- The installation may take up to an hour, depending on selected features.

**Image**  
![Flare-VM Feature Selection](path/to/image3.png)

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
![pfSense Network Adapter Setup](path/to/image4.png)

---

### 5. Installing pfSense
- Start the pfSense VM and follow the installation process (just press enter through defaults).
- After installation, reboot the VM and unmount the ISO.
- Boot up pfSense and verify the setup screen.

**Image**  
![pfSense Setup Screen](path/to/image5.png)

---

### 6. Importing Remnux
- Download and import the Remnux OVA into VirtualBox.
- Set the network adapter for Remnux to the **host-only** adapter created earlier.

**Image**  
![Remnux Import and Network Configuration](path/to/image6.png)

---

### 7. Configuring Flare-VM for Isolation
- Once Flare-VM installation is complete, switch its network adapter to the **host-only** adapter.
- Access the pfSense web GUI via the default gateway and configure the LAN interface and DNS settings.

**Image**  
![Flare-VM Connected to pfSense](path/to/image7.png)

---

### 8. Configuring Firewall Rules in pfSense
- Go to `Firewall > Rules` and create rules to prevent the VM from accessing your local network while maintaining internet connectivity.

**Image**  
![Firewall Rules in pfSense](path/to/image8.png)

---

### 9. Setting Up Remnux for Malware Analysis
- Boot up Remnux and set the network configuration to match the pfSense LAN IP.
- Start the **fakedns** service for simulating DNS traffic during analysis.

**Image**  
![Remnux DNS Configuration](path/to/image9.png)

---

### 10. Downloading and Testing Malware
- Download a malware sample like **WannaCry** from [MalwareZoo](https://github.com/abuisa/MalwareZoo).
- Before running the malware, switch the Flare-VM to the Remnux environment for isolated testing.

**Image**  
![Malware Execution in Flare-VM](path/to/image10.png)

---

### 11. Restoring VM Snapshots
- Take snapshots of your VM before malware detonation for easy restoration.
- After testing, restore the snapshot to reset your environment.

**Image**  
![Snapshot Management in VirtualBox](path/to/image11.png)

---

## Additional Notes
- Always ensure proper isolation when testing malware.
- Keep your snapshots up to date to minimize downtime when restoring VMs.

---

## Conclusion
This guide sets up a secure and isolated environment for malware analysis using Windows 10, Flare-VM, pfSense, and Remnux. More advanced labs and exercises will follow!
