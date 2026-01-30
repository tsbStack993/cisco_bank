**1‑month project schedule** 
break it into **weekly milestones** 

---

##  One‑Month Project Schedule

### **Week 1 – Planning & Design**
- Define requirements: VLANs, departments, services (ATM, Staff, Finance, Management, IT, Security, Lobby).  
- Draw a **flowchart/system diagram** with VLANs, switches, servers, and router/firewall.  
- Assign IP addressing scheme (e.g., 192.168.X.0/24 per VLAN).  
- Decide ACL rules (who can access which server).  
- Deliverable: **Network design document + flowchart**.

---

### **Week 2 – Basic Topology Setup**
- Build topology in Packet Tracer:  
  - PCs for ATM, Staff, Finance, Management, IT, Security, Lobby.  
  - Layer 2 switches for access.  
  - Layer 3 switch for routing.  
  - Router/firewall for internet access.  
- Configure VLANs and assign ports.  
- Configure SVIs (gateway IPs) on Layer 3 switch.  
- Deliverable: **Working topology with VLAN segmentation**.

---

### **Week 3 – Server Configuration & ACLs**
- Add servers in VLAN 50 (Data Center).  
  - Transaction Server (HTTP).  
  - Database Server (DNS/FTP).  
  - Authentication Server (DHCP/DNS).  
  - ERP/File Server (FTP).  
  - CCTV Server (FTP/TFTP).  
- Configure services on servers.  
- Apply ACLs on Layer 3 switch/router:  
  - ATM → Transaction Server only.  
  - Staff → ERP/File + Finance.  
  - Finance → Database only.  
  - Lobby → Internet only.  
  - Security → CCTV server only.  
  - Management & IT → Full access.  
- Deliverable: **Servers reachable according to ACL rules**.

---

### **Week 4 – Testing & Documentation**
- Test connectivity:  
  - Ping between VLANs.  
  - Access servers from correct VLANs.  
  - Verify Lobby VLAN cannot reach internal servers.  
- Test internet access via router/firewall.  
- Document configuration: VLANs, IPs, ACLs, services.  
- Deliverable: **Final Packet Tracer project file + documentation**.

---

## Outcome After 1 Month
- A fully segmented **bank network simulation** in Packet Tracer.  
- Clear **flowchart + documentation** for reference.  
- Tested ACL rules ensuring **security and proper access control**.  
- Internet connectivity simulated through router/firewall.  

---
