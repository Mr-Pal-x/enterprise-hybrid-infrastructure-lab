# Enterprise Hybrid Infrastructure & Identity Lab

Production-modeled enterprise environment showcasing hybrid systems and cloud identity architecture, featuring virtualized Windows/Linux infrastructure, enterprise directory services, federated SSO, and infrastructure automation.

---

## Target Core Competencies

* **Directory Services & Identity Management:** Windows Server 2022 (`DC01`), Active Directory Domain Services (AD DS), Tiered Organizational Units (OUs), Group Policy Objects (GPO), and Role-Based Access Control (RBAC).
* **Enterprise Network & Perimeter Security:** OPNsense virtual firewall/router, network isolation (`10.0.10.0/24`), Split-DNS architecture, and Tailscale zero-trust remote access overlays.
* **Enterprise Linux Administration:** Red Hat Enterprise Linux (RHEL) / AlmaLinux deployment, SSSD/Realm domain integration, and system hardening.
* **Hybrid Cloud & SSO:** Microsoft Entra ID (Azure AD Connect) identity synchronization and Keycloak OIDC/OAuth 2.0 federated authentication.
* **Infrastructure Automation:** Ansible, PowerShell, Python, and Bash scripting for administrative lifecycle management and cross-platform orchestration.

---

## Technical Architecture

* **Hypervisor Platform:** Proxmox VE (Bare-metal virtualized infrastructure with Linux bridge WAN/LAN interfaces)
* **Perimeter Firewall & Routing:** OPNsense VM (`10.0.10.1`)
* **Identity Core:** Windows Server 2022 (`DC01` - Primary Domain Controller)
* **Linux Environment:** Red Hat Enterprise Linux (RHEL) / AlmaLinux Control Node
* **Network & DNS:** Isolated `10.0.10.0/24` subnet + Active Directory Split-DNS pipeline (`127.0.0.1` AD loopback + upstream recursive resolvers)
* **Secure Access:** Out-of-band encrypted management overlay via OPNsense Tailscale plugin (No public NAT port forwarding)
* **Hybrid Cloud Extension:** Microsoft Entra ID & Keycloak OIDC

---

## Network Topography

| Device / VM | Role | Subnet / IP | Management Access |
| :--- | :--- | :--- | :--- |
| **OPNsense** | Virtual Gateway / Firewall | `10.0.10.1` | Tailscale / Web GUI |
| **DC01** | Active Directory / DNS | `10.0.10.x` | SSH / Remote Port
| **RHEL Node** | Ansible Control Node | `10.0.10.x` | SSH / Console |

---

## Operational Implementation Log

### Phase 1: Core Directory & Infrastructure Baseline (Current)
- [x] Bare-metal Proxmox VE deployment & VirtIO driver acceleration
- [x] Virtualized network isolation via OPNsense firewall & dedicated LAN bridge interface (`10.0.10.0/24`)
- [x] Zero-trust out-of-band remote management via Tailscale plugin on OPNsense
- [x] Windows Server 2022 deployment & root Domain Controller promotion (`DC01`)
- [x] Split-DNS architecture configuration (AD local lookup + enterprise upstream resolution)
- [x] Disaster recovery baseline verified via Proxmox snapshot (`DC01-Post-AD-DNS-Success`)
- [ ] Tiered Organizational Unit (OU) structure & Role-Based Access Control (RBAC) implementation
- [ ] Group Policy Baseline (GPO) deployment

### Phase 2: Automation & Enterprise Linux Integration
- [ ] Automated bulk user/OU provisioning via PowerShell scripts
- [ ] WinRM HTTPS over port 5986 configuration & Ansible Vault encrypted inventory setup
- [ ] Red Hat Enterprise Linux (RHEL) server deployment & AD domain integration (SSSD)
- [ ] Python-based infrastructure monitoring scripts

### Phase 3: Hybrid Cloud & Federated Identity
- [ ] Microsoft Entra ID (Azure AD Connect) hybrid identity synchronization
- [ ] Keycloak federated OIDC/OAuth single sign-on deployment
