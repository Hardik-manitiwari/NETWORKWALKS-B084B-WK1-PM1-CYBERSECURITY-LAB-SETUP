# Cybersecurity & Ethical Hacking Lab Setup Guide 🧪

Welcome! This repository provides step-by-step instructions to create an isolated, secure, and fully functional cybersecurity testing environment on your personal computer using **VirtualBox** and **Kali Linux**.

Whether you're starting out in ethical hacking or setting up a personal lab for CTFs (Capture The Flag) and network analysis, this guide will walk you through the entire process.

---

## 📌 System Requirements

To ensure a smooth experience running virtual machines simultaneously, the following system specifications are recommended:

* **RAM:** 8 GB or more
* **Storage:** 256 GB SSD or more
* **Processor:** Intel Core i3 / i5 (or equivalent AMD processor with virtualization enabled)
* **Host Operating System:** Windows 10/11, macOS, or Linux

---

## 🌐 Network Architecture Overview

All virtual machines will reside inside an isolated VirtualBox custom **NAT Network** using the `10.0.0.0/24` subnet.

```
       +-------------------------------------------------+
       |                  Host OS                        |
       |         (e.g., Windows 10 / 11)                 |
       +------------------------+------------------------+
                                |
                    +-----------v-----------+
                    | VirtualBox Manager    |
                    | (NAT Network:         |
                    |  10.0.0.0/24)         |
                    +-----------+-----------+
                                |
   +----------------------------+----------------------------+
   |                            |                            |
+--v------------------+  +------v---------------+  +---------v------------+
| Kali Linux          |  | Windows 10 / 11      |  | Target / CTF VMs     |
| (Attacker Machine)  |  | (Target Machine)     |  | (Optional Targets)   |
| IP: 10.0.0.2/24     |  | IP: 10.0.0.10/24     |  | IP: 10.0.0.x/24      |
+---------------------+  +----------------------+  +----------------------+
```

### IP Address Allocation Table

| Machine / Target | Subnet / Network | IP Address | Purpose |
| :--- | :--- | :--- | :--- |
| **Default Gateway** | NAT Network | `10.0.0.1` | Network routing |
| **Kali Linux** | NAT Network | `10.0.0.2/24` | Primary Attacking/Hacker Machine |
| **Windows 11** | NAT Network | `10.0.0.7/24` | Target System (Optional) |
| **Android 9.0** | NAT Network | `10.0.0.9/24` | Mobile Target (Optional) |
| **Windows 10** | NAT Network | `10.0.0.10/24` | Target System (Optional) |
| **Windows 7** | NAT Network | `10.0.0.11/24` | Legacy Target (Optional) |
| **Windows Server 2016** | NAT Network | `10.0.0.16/24` | Active Directory Target (Optional) |

---

## 🚀 Setup Steps

### Phase 1: Basic Setup & Attacker Machine

#### Step 1: Install Prerequisites
1. Download and install **7-Zip** (or your preferred archive extractor) from [7-zip.org](https://7-zip.org/download.html).
2. Download and install the latest version of **Oracle VirtualBox** from [virtualbox.org](https://virtualbox.org/wiki/Downloads).

#### Step 2: Configure VirtualBox NAT Network
1. Open **Oracle VirtualBox Manager**.
2. Go to **Tools** > **Network** > **NAT Networks** tab.
3. Click **Create** to add a new network (e.g., `NatNetwork`).
4. Set the **IPv4 Prefix** to `10.0.0.0/24`.
5. Ensure **Enable DHCP** is checked, then click **Apply**.

#### Step 3: Import & Configure Kali Linux VM
1. Download the pre-built Kali Linux VM for VirtualBox from [kali.org/get-kali](https://kali.org/get-kali).
2. Import the `.ova` file into VirtualBox (**File** > **Import Appliance...**).
3. Open the **Settings** for the Kali Linux VM:
   * **General > Advanced:** Set **Shared Clipboard** and **Drag'n'Drop** to `Bidirectional`.
   * **Network > Adapter 1:** Select **Attached to: NAT Network** and set **Name: NatNetwork**. Under Advanced, set **Promiscuous Mode** to `Allow All`.
   * **Shared Folders:** Add a shared folder pointing to your host machine's `/Downloads` folder for easy file exchange.

#### Step 4: Configure IP Settings in Kali Linux
1. Start the Kali Linux VM.
2. Go to **Network Settings** or edit the active connection (**Wired connection 1**):
   * Set **IPv4 Method** to `Manual`.
   * **Address:** `10.0.0.2`
   * **Netmask:** `24` (or `255.255.255.0`)
   * **Gateway:** `10.0.0.1`
   * **DNS Servers:** `8.8.8.8` (or `10.0.0.1`)
3. Save changes and reconnect the network interface.

#### Step 5: Save a Baseline Snapshot
Before running tools or modifying system files, create a clean state snapshot:
* Go to **Machine** > **Take Snapshot...** and name it `Baseline / Clean Install`.

---

### Phase 2: Target Machines Setup (Optional / Future Labs)

1. Import additional target operating systems (e.g., Windows 10/11, Windows Server, or Android).
2. Attach each VM’s network interface to the same `NatNetwork`.
3. Assign manual IP addresses within the `10.0.0.0/24` subnet according to the IP Allocation Table above.
4. Verify connectivity between machines using `ping`.
5. Take a snapshot of each target machine once configured.

---

## 🔧 Troubleshooting Connectivity Issues

If your Kali Linux VM or target machine loses internet access on VirtualBox 7+ or newer Kali versions, follow these steps:

### Common Workaround via Command Line
Run the following commands in the Kali Linux terminal to modify NetworkManager settings:

```bash
# Disable DAD (Duplicate Address Detection) timeout
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

# Bring the connection down and back up
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

### Checklist for No Internet Connection:
1. Verify `NatNetwork` exists in VirtualBox with the `10.0.0.0/24` prefix.
2. Confirm no two VMs share the same static IP address on the subnet.
3. Check that the virtual network cable is marked as connected in the VM settings.
4. Restart the VirtualBox service or reboot both the host machine and guest VMs.

---

## 💬 Community & Support

* For further course modules and technical guides, visit [Network Walks Academy](https://www.networkwalks.com).
* If you encounter issues during setup, feel free to share your queries in the comments of the module video guide or contact `info@networkwalks.com`.
