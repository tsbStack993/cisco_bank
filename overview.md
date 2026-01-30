##  Flow Overview
- **Ground Floor**  
  - ATM (VLAN 10) → Transaction Server  
  - Lobby (VLAN 30) → Internet only  

- **First Floor**  
  - Staff (VLAN 20) → ERP/File Server  
  - Finance (VLAN 60) → Database Server  

- **Second Floor**  
  - Management (VLAN 40) → Full access  
  - Server/Data Center (VLAN 50) → Hosts all services  
  - IT Admin (VLAN 70) → Full access for administration  
  - Security (VLAN 80) → CCTV/Video storage server  

---

##  Connectivity
- **Layer 2 switches** on each floor connect PCs/ATMs to VLANs.  
- **Layer 3 switch** interconnects all VLANs and enforces ACL rules.  
- **Core Router/Firewall** connects the Layer 3 switch to the **Internet (WAN/ISP)**.  
- **Servers in VLAN 50** provide:  
  - Transaction (HTTP)  
  - Database (DNS/FTP)  
  - Authentication (DHCP/DNS)  
  - ERP/File (FTP)  
  - Security/CCTV (FTP/TFTP)  

---

##  Flow Logic
- ATM VLAN → Transaction Server only  
- Staff VLAN → ERP/File + Finance systems  
- Finance VLAN → Database + Server VLAN only  
- Lobby VLAN → Internet only (no internal access)  
- Management & IT VLANs → Full access  
- Security VLAN → CCTV server only  

---
