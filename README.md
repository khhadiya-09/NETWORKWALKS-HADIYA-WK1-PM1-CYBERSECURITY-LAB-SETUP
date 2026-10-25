<div align="center">

# Cybersecurity Lab Setup

**Networkwalks · Week 1 · Project Module 1**

A documented Kali Linux lab environment built with Oracle VirtualBox for cybersecurity learning and authorized testing.

</div>

---

## About the Project

This project documents my setup of a virtual cybersecurity lab using **Oracle VirtualBox** and **Kali Linux**. It covers NAT Network configuration, static IPv4 addressing, connectivity checks, shared-folder setup, and troubleshooting encountered during configuration.

The goal is to prepare a repeatable environment for future cybersecurity labs and authorized security-testing exercises.

## Environment and Configuration

| Component | Configuration |
|---|---|
| Host operating system | Windows |
| Virtualization platform | Oracle VirtualBox 7.2.14 |
| Virtual machine | Kali Linux 2026.2 |
| Network mode | NAT Network |
| NAT Network name | `NatNetwork` |
| Network prefix | `10.0.0.0/24` |
| Kali IPv4 address | `10.0.0.2/24` |
| Default gateway | `10.0.0.1` |
| DNS configured in Kali | `8.8.8.8` |
| Shared folder | Windows Downloads, mounted in Kali at `/media/sf_downloads` |

## Screenshots

Keep the `screenshots/` folder in the repository alongside this README. The image links below use relative paths, so the screenshots should render when the folder and filenames are uploaded unchanged.

### 1. VirtualBox NAT Network

The NAT Network is named `NatNetwork`, uses the IPv4 prefix `10.0.0.0/24`, and has DHCP enabled.

![VirtualBox NAT Network settings](screenshots/nat-network-settings.png)

### 2. Kali Linux and Shared Folder

The `sf_downloads` shared-folder shortcut is visible on the Kali desktop.

![Kali desktop showing the shared folder](screenshots/kali-desktop-shared-folder.png)

### 3. Kali Static IPv4 Settings

The NetworkManager profile is configured with the static IPv4 address, gateway, and DNS server shown below.

![Kali static IPv4 settings](screenshots/kali-static-ip-settings.png)

## Setup Steps

1. Installed Oracle VirtualBox and imported the Kali Linux virtual machine.
2. Created a NAT Network named `NatNetwork` with the IPv4 prefix `10.0.0.0/24`.
3. Connected the Kali VM's network adapter to the NAT Network.
4. Configured `Wired connection 1` in Kali with:
   - **Address:** `10.0.0.2/24`
   - **Gateway:** `10.0.0.1`
   - **DNS:** `8.8.8.8`
5. Enabled Bidirectional mode for Shared Clipboard and Drag and Drop in VirtualBox settings.
6. Added the Windows Downloads folder as a VirtualBox shared folder with auto-mount enabled.
7. Tested access to the shared folder and confirmed that a file created in Kali appeared in Windows.

## Verification

| Check | Observed result |
|---|---|
| Kali IP and route | `10.0.0.2/24`; default route via `10.0.0.1` |
| Gateway connectivity | `ping -c 4 10.0.0.1` — 4 replies, 0% packet loss |
| Internet connectivity by IP | `ping -c 4 1.1.1.1` — 4 replies, 0% packet loss |
| DNS and connectivity | `ping -c 4 google.com` resolved the domain and received 4 replies |
| Shared-folder mount | Available at `/media/sf_downloads` |
| Shared-folder write test | Created `kali_share_test.txt` in Kali and confirmed it appeared in Windows Downloads |
| Shared Clipboard | Copy-paste tested successfully |
| Drag and Drop | Enabled in settings, but file transfer still displays “Can't drop data to guest”; unresolved |

## Troubleshooting: DHCP Address Conflict

### Issue

When I first configured Kali to use `10.0.0.2/24`, NetworkManager reported that the address was already in use. The NAT Network's DHCP server was also configured to use `10.0.0.2`, conflicting with the static address required for Kali.

### Resolution

I modified the VirtualBox NAT Network DHCP server configuration from Windows using `VBoxManage`. The DHCP server address was changed to `10.0.0.254`, and its lease range was set to `10.0.0.3`–`10.0.0.253`, leaving `10.0.0.2` available for Kali.

After that change, I configured Kali with the static address and verified gateway connectivity, internet access, DNS resolution, and shared-folder access.

## What I Learned

- How a VirtualBox NAT Network differs from a basic NAT adapter.
- How to configure and verify a static IPv4 address, gateway, and DNS.
- How to troubleshoot an IP address conflict between a DHCP server and a static VM address.
- How to configure and test a VirtualBox shared folder.
- Why connectivity tests and clear technical documentation matter when preparing a lab.

## Current Status

- [x] VirtualBox and Kali VM configured
- [x] NAT Network configured
- [x] Kali static IP configured and verified
- [x] Gateway, internet, and DNS tests passed
- [x] Shared-folder read/write test passed
- [x] Shared Clipboard tested
- [x] Create a clean VirtualBox snapshot 
- [x] Investigated the drag-and-drop issue; transfer remains unresolved

## Responsible Use

This lab is intended for education and authorized cybersecurity testing only. Test only systems you own or have explicit permission to assess.

## Author

**Hadiya Khan**  
Cybersecurity student | Computer Science Engineering (IoT, Cybersecurity and Blockchain Technology)

- **LinkedIn:** [linkedin.com/in/hadiya-khan-3aba54288](https://www.linkedin.com/in/hadiya-khan-3aba54288)

---

*Networkwalks · Week 1 · Project Module 1 — Cybersecurity Lab Setup*
