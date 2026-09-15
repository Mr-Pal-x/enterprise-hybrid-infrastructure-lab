# Phase 1.2: Virtual Machine Provisioning & Storage Configuration

##  Implementation Summary
Provisioned primary infrastructure Virtual Machines (Windows Server 2022 and Red Hat Enterprise Linux) on Proxmox VE. Implemented enterprise-grade storage driver baselines for Windows guests and RHCSA-aligned manual disk partitioning for RHEL nodes.

---

## 🪟 Windows Server 2022 Deployment (`DC01`)

### Storage Driver Architecture & VirtIO Integration
* **Configuration:** Provisioned guest VM using Proxmox SCSI controller (`virtio-scsi-pci`) to maximize storage I/O performance over legacy IDE/SATA emulation.
* **Troubleshooting Log — Missing Storage Target:**
  * **Issue:** During the Windows Server 2022 installation wizard, no available disk drives were detected on the storage selection screen.
  * **Root Cause:** Windows Server media lacks native out-of-box drivers for High-Performance VirtIO SCSI disk controllers.
  * **Resolution Procedure:**
    1. Mounted the VirtIO ISO (`virtio-win.iso`) as a secondary virtual CD/DVD drive in Proxmox VM hardware settings.
    2. Selected **Load Driver** within the Windows Setup interface.
    3. Navigated to the mounted drive: `\vioscsi\2k22\amd64` (VirtIO SCSI driver path for Server 2022).
    4. Loaded the `vioscsi` driver into memory, instantly exposing the virtual hard disk target for partition creation and OS installation.

---

## 🐧 Enterprise Linux Deployment (RHEL Node)

### Manual Storage Partitioning (RHCSA-Aligned Baseline)
Bypassed automatic disk partitioning during RHEL installation to construct a custom storage topology aligned with Enterprise Linux administration baselines:

* **`/boot` Partition:** Dedicated boot partition allocated to isolate system bootloaders and kernel images.
* **`/` (Root Directory):** Base OS installation file system.
* **`swap` Partition:** Virtual memory swap file system provisioned for memory management stability.
* **`/var` Partition:** Isolated partition for system logging, spooling, and service runtime data to prevent root storage exhaustion during unexpected log growth.

---

##  Outcome
* **Storage Optimization:** Windows Server operating on high-efficiency VirtIO storage drivers with minimal host overhead.
* **Linux Hardening:** Custom RHEL storage layout mitigating storage-exhaustion risks and modeling RHCSA file system management standards.
