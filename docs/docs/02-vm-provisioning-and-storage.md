# Phase 1.2 & 1.3: VM Provisioning, Virtual Networking & Identity Core Baseline

## Implementation Summary
Provisioned core virtual machines (OPNsense Firewall, Windows Server 2022, and Red Hat Enterprise Linux) on Proxmox VE. Constructed a dual-interface virtual network architecture to segment lab traffic from the primary home network. Resolved storage and network driver dependencies using VirtIO guest media, initialized system identity configurations, and promoted the primary Windows Server node to Root Domain Controller (`DC01`).

---

## Virtual Network Architecture & Perimeter Security (OPNsense)

### 1. Dual-Interface Bridge & Network Segmentation
* **Configuration:** Provisioned a virtualized OPNsense firewall appliance acting as the primary gateway, DHCP, and DNS router for the lab environment.
* **Network Topography:**
  * **WAN Interface:** Bridged to the physical Proxmox host NIC for upstream internet routing.
  * **LAN Interface:** Dedicated virtual Linux bridge (`vmbr1`) hosting an isolated `10.0.10.0/24` internal subnet.

### 2. Troubleshooting Log — Remote Management & Secure Ingress
* **Symptom:** Needed secure remote management (RDP/Remmina, SSH, Web GUIs) across the newly isolated `10.0.10.0/24` subnet without exposing sensitive lab infrastructure to the public internet.
* **Initial Consideration:** Exposing services via standard NAT port forwarding on the perimeter firewall.
* **Security Risk & Resolution:** Rejected public NAT port forwarding due to severe security vulnerabilities. Implemented the official **Tailscale plugin directly on OPNsense** to establish a zero-trust, out-of-band encrypted mesh network overlay.
* **Verification:** Successfully accessed lab management interfaces (Remmina RDP to `DC01`, SSH to RHEL, and OPNsense Web UI) remotely via Tailscale overlay IPs without opening external firewall ports.

---

## Windows Server 2022 Deployment & Domain Controller Promotion (`DC01`)

### 1. Storage & Network Driver Troubleshooting
* **Configuration:** Assigned VirtIO SCSI disk controller (`virtio-scsi-pci`) and VirtIO Paravirtualized Network Interface Card (`VirtIO / NetKVM`) for maximum I/O performance and low CPU overhead.
* **Troubleshooting Log — Missing Storage Target:**
  * **Symptom:** Windows Setup reported "No drives were found" during disk selection.
  * **Root Cause:** Lack of native out-of-box VirtIO storage drivers in standard Windows Server ISO media.
  * **Resolution:** Mounted `virtio-win.iso` as a secondary virtual optical drive, selected **Load Driver**, and navigated to `\vioscsi\2k22\amd64` to load the VirtIO SCSI driver.
* **Troubleshooting Log — No Network Interface Detected:**
  * **Symptom:** Server booted with no active network connectivity; Device Manager flagged the Ethernet Controller as an unrecognized device.
  * **Root Cause:** Windows Server 2022 lacks native drivers for Red Hat / VirtIO paravirtualized NIC adapters.
  * **Resolution:** Opened Device Manager, initiated a driver update for the network interface targeting the mounted VirtIO ISO directory (`\NetKVM\2k22\amd64`), and installed the Red Hat VirtIO Ethernet Adapter driver to restore network stack functionality.

### 2. Base OS Identity & Domain Role Promotion
* **Hostname Standardization:** Renamed default system name to enterprise standard hostname **`DC01`**.
* **IP Migration:** Re-assigned network configuration from legacy home subnet to the new OPNsense internal subnet (`10.0.10.x`).
* **Role Installation:** Installed **Active Directory Domain Services (AD DS)** and **DNS Server** roles via Server Manager.
* **Forest Initialization:** Executed Domain Controller promotion wizard to create a new Active Directory forest (`lab.internal` / enterprise root domain).
* **Directory Services Restore Mode (DSRM):** Configured isolated DSRM administrative credential baseline for emergency directory recovery.

---

## Enterprise Linux Deployment (RHEL Node)

### Manual Storage Partitioning (RHCSA-Aligned Baseline)
Bypassed default automatic partitioning during RHEL installation to construct a custom storage topology aligned with Enterprise Linux administration baselines:

* **`/boot` Partition:** Dedicated boot partition allocated to isolate system bootloaders and kernel images.
* **`/` (Root Directory):** Base OS installation file system.
* **`swap` Partition:** Virtual memory swap file system provisioned for memory management stability.
* **`/var` Partition:** Isolated partition for system logging, spooling, and service runtime data to prevent root storage exhaustion during unexpected log growth.

---

## Outcomes
* **Network Isolation:** Lab workloads fully segregated onto an isolated `10.0.10.0/24` subnet behind an OPNsense virtual gateway.
* **Zero-Trust Access:** Secure remote administration established via Tailscale mesh VPN, eliminating the need for hazardous NAT port forwards.
* **Hardware Acceleration:** Windows Server fully operational on high-efficiency VirtIO storage (`vioscsi`) and network (`NetKVM`) paravirtualized drivers.
* **Identity Core Active:** Domain Controller `DC01` established as the root identity provider and primary DNS authority within the isolated lab network.
* **Linux Hardening:** Custom RHEL storage layout mitigating storage-exhaustion risks and modeling RHCSA file system management standards.
