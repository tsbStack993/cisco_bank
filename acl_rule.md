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


ATM (VLAN 10)
Only talk to Transaction Server
Nothing else

Lobby (VLAN 20)
Internet only
No access to internal servers

Management (VLAN 30)
Full access

IT (VLAN 40)
Full access for maintenance

Finance (VLAN 50)
ERP + Database only

 Staff (VLAN 60)
ERP only

 Security (VLAN 70)
Access to everything for monitoring

Servers (VLAN 80)
No direct access to user VLANs, 

192.168.80.12 = Transaction Server
192.168.80.13 = Database
192.168.80.14 = ERP
