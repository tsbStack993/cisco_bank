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

conf inte

```
interface g0/0
nameif inside
security-level 100
ip address 10.0.0.6 255.255.255.252
no shutdown

interface g0/1
nameif outside
security-level 0
ip address 203.0.0.1 255.255.255.0
no shutdown

! route to outside
route outside 0.0.0.0 0.0.0.0 203.0.0.2

!route to internal
route inside 192.168.10.0 255.255.255.0 10.0.0.5
route inside 192.168.50.0 255.255.255.0 10.0.0.5

!conf net
object network LAN10
subnet 192.168.10.0 255.255.255.0
nat (inside,outside) dynamic interface

object network LAN50
subnet 192.168.50.0 255.255.255.0
nat (inside,outside) dynamic interface

!internet server
203.0.0.2
Mask: 255.255.255.0
Gateway: 203.0.0.1

!on rotuer
ip route 0.0.0.0 0.0.0.0 10.0.0.6

access-list outside_access_in extended permit icmp any any
!
!
access-group outside_access_in in interface outside


route inside_name 192.168.10.0 255.255.255.0 10.0.0.5
ip route 0.0.0.0 0.0.0.0 10.0.0.6 in router
```
