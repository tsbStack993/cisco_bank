ATM → l2 switch → l3 switch → Core L3 → Transaction Server (VLAN 50)
BLOCK → All other internal VLANs + Internet

Lobby PC → Firewall → Internet
BLOCK → All internal VLANs

Staff → ERP/File Server
Staff → Finance Applications
BLOCK → Direct DB access

Finance → Database Server
Finance → ERP Server
BLOCK → Other user VLANs

Management → All VLANs + Internet

IT Admin → All VLANs (SSH, RDP, SNMP)

CCTV Cameras → CCTV Server only
BLOCK → Internet & user VLANs
