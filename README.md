# Cybersecurity Lab Setup — Networkwalks Week 1

A personal lab-setup project documenting my Kali Linux virtual machine and network configuration in Oracle VirtualBox. The goal is to prepare a controlled environment for future cybersecurity learning and authorized lab exercises.

## Project goals

- Configure Oracle VirtualBox for a cybersecurity lab.
- Create a NAT Network for the lab.
- Configure Kali Linux with a static IPv4 address.
- Verify gateway access, internet connectivity, and DNS resolution.
- Share files between Windows and Kali Linux.
- Record setup issues and the steps used to resolve them.

## Environment

| Component | Configuration |
|---|---|
| Host operating system | Windows |
| Virtualization software | Oracle VirtualBox 7.2.14 |
| Virtual machine | Kali Linux 2026.2 |
| Virtual network mode | NAT Network |
| NAT Network name | `NatNetwork` |
| IPv4 network | `10.0.0.0/24` |
| Kali IPv4 address | `10.0.0.2/24` |
| Default gateway | `10.0.0.1` |
| DNS server configured in Kali | `8.8.8.8` |
| Shared folder | Windows Downloads, mounted in Kali at `/media/sf_downloads` |

## Screenshots

### 1. NAT Network configuration

The VirtualBox NAT Network is configured with the IPv4 prefix `10.0.0.0/24` and DHCP enabled.

![VirtualBox NAT Network settings](screenshots/01-nat-network-settings.png)

### 2. Kali desktop and shared folder

The `sf_downloads` shared-folder shortcut is visible on the Kali desktop.

![Kali desktop with shared folder](screenshots/02-kali-desktop-and-shared-folder.png)

### 3. Kali static IPv4 configuration

The active NetworkManager profile is configured manually with the Kali address, gateway, and DNS server shown below.

![Kali IPv4 settings](screenshots/03-kali-static-ip-settings.png)

## Setup summary

1. Installed Oracle VirtualBox and imported the Kali Linux virtual machine.
2. Created a NAT Network named `NatNetwork` with the IPv4 prefix `10.0.0.0/24`.
3. Connected the Kali VM's network adapter to that NAT Network.
4. Configured Kali's `Wired connection 1` with:
   - Address: `10.0.0.2/24`
   - Gateway: `10.0.0.1`
   - DNS: `8.8.8.8`
5. Enabled bidirectional shared clipboard and drag-and-drop in VirtualBox settings.
6. Added the Windows Downloads folder as a VirtualBox shared folder with auto-mount enabled.
7. Verified that Kali can access the shared folder and create a test file that appears in Windows.

## Verification performed

| Check | Result |
|---|---|
| Kali address and route | Confirmed `10.0.0.2/24` with default route via `10.0.0.1` |
| Gateway connectivity | `ping -c 4 10.0.0.1` — 4 replies, 0% packet loss |
| Internet connectivity by IP | `ping -c 4 1.1.1.1` — 4 replies, 0% packet loss |
| DNS resolution and connectivity | `ping -c 4 google.com` — name resolved and 4 replies received |
| Shared folder mounted | Confirmed at `/media/sf_downloads` |
| Shared-folder write test | Created `kali_share_test.txt` in Kali and confirmed it appeared in Windows Downloads |
| Clipboard | Copy-paste tested successfully |
| Drag-and-drop | Enabled in settings, but file transfer still reports “Can't drop data to guest”; unresolved |

## Troubleshooting: DHCP address conflict

### Problem

When I first tried to set Kali to `10.0.0.2/24`, NetworkManager reported that the address was already in use. The VirtualBox NAT Network DHCP server was also using `10.0.0.2`, which conflicted with the required static address for Kali.

### Resolution

I updated the NAT Network DHCP server configuration from Windows using `VBoxManage`, moving its server address to `10.0.0.254` and limiting the lease range to `10.0.0.3`–`10.0.0.253`. This left `10.0.0.2` available for Kali's static address.

I then configured Kali with the static address and re-tested the gateway, internet access, DNS, and shared-folder access.

> The DHCP range and static address should be checked against the actual VirtualBox network configuration before reusing these settings in another lab.

## Current status and next steps

- [x] VirtualBox and Kali configured
- [x] NAT Network configured
- [x] Kali static IPv4 configuration verified
- [x] Internet and DNS connectivity verified
- [x] Shared folder access verified in both Windows and Kali
- [x] Clipboard tested
- [ ] Create a clean VirtualBox snapshot after confirming the baseline
- [ ] Continue investigating the drag-and-drop issue

## Safe and responsible use

This environment is intended for education and authorized cybersecurity testing only. Test only systems that you own or have explicit permission to assess.

## Acknowledgement

Prepared as my own Week 1 lab-setup documentation for the Networkwalks cybersecurity program. The configuration, troubleshooting notes, and screenshots reflect my setup experience.
