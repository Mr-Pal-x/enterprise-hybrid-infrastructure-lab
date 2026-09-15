# Enterprise Hybrid Infrastructure & Identity Lab

Production-modeled enterprise environment showcasing hybrid systems and cloud identity architecture, featuring virtualized Windows/Linux infrastructure, enterprise directory services, federated SSO, and infrastructure automation.

---

## 🏛️ Target Core Competencies

* **Directory Services & Identity Management:** Windows Server 2022 (`DC01`), Active Directory Domain Services (AD DS), Tiered Organizational Units (OUs), Group Policy Objects (GPO), and Role-Based Access Control (RBAC).
* **Enterprise Linux Administration:** Red Hat Enterprise Linux (RHEL) / AlmaLinux deployment, SSSD/Realm domain integration, and system hardening.
* **Hybrid Cloud & SSO:** Microsoft Entra ID (Azure AD Connect) identity synchronization and Keycloak OIDC/OAuth 2.0 federated authentication.
* **Network & Security Engineering:** Split-DNS architecture, enterprise forwarder resolution, and Tailscale zero-trust remote access overlays.
* **Infrastructure Automation:** PowerShell, Python, and Bash scripting for administrative lifecycle management and automated system configuration.

---

## 🏗️ Technical Architecture

* **Hypervisor Platform:** Proxmox VE (Bare-metal virtualized infrastructure)
* **Identity Core:** Windows Server 2022 (`DC01` - Primary Domain Controller)
* **Linux Environment:** Red Hat Enterprise Linux (RHEL) / AlmaLinux Node
* **Network & DNS:** Active Directory Split-DNS pipeline (`127.0.0.1` AD loopback + upstream recursive resolvers)
* **Secure Access:** Tailscale out-of-band encrypted management overlay
* **Hybrid Cloud Extension:** Microsoft Entra ID & Keycloak OIDC

---

## 📋 Operational Implementation Log

### Phase 1: Core Directory & Infrastructure Baseline (Current)
- [x] Bare-metal Proxmox VE deployment & VirtIO driver acceleration
- [x] Windows Server 2022 deployment & root Domain Controller promotion (`DC01`)
- [x] Split-DNS architecture configuration (AD local lookup + enterprise upstream resolution)
- [x] Zero-trust out-of-band remote management via Tailscale overlay
- [x] Disaster recovery baseline verified via Proxmox snapshot (`DC01-Post-AD-DNS-Success`)
- [ ] Tiered Organizational Unit (OU) structure & Role-Based Access Control (RBAC) implementation
- [ ] Group Policy Baseline (GPO) deployment

### Phase 2: Automation & Enterprise Linux Integration
- [ ] Automated bulk user/OU provisioning via PowerShell scripts
- [ ] Red Hat Enterprise Linux (RHEL) server deployment & AD domain integration (SSSD)
- [ ] Python-based infrastructure monitoring scripts

### Phase 3: Hybrid Cloud & Federated Identity
- [ ] Microsoft Entra ID (Azure AD Connect) hybrid identity synchronization
- [ ] Keycloak federated OIDC/OAuth single sign-on deployment
