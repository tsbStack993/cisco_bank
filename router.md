Layer 3 switches/router.

### 1. VLANs on L2 switches
On left L2 switch (PC1, PC2 side):
- Create VLAN 10 and VLAN 20.  
- Set PC1 port as access vlan 10.  
- Set PC2 port as access vlan 20.  
- Set uplink to L3 switch as trunk (allow VLANs 10,20).

On right L2 switch (PC3, PC4 side):
- Create VLAN 30 and VLAN 40.  
- Set PC3 port as access vlan 30.  
- Set PC4 port as access vlan 40.  
- Set uplink to L3 switch as trunk (allow VLANs 30,40).

### 2. SVIs on both L3 switches
On left L3 switch:
- Create interface VLAN 10 with IP (e.g. 192.168.10.1/24).  
- Create interface VLAN 20 with IP (e.g. 192.168.20.1/24).  
- Enable IP routing.

On right L3 switch:
- Create interface VLAN 30 with IP (e.g. 192.168.30.1/24).  
- Create interface VLAN 40 with IP (e.g. 192.168.40.1/24).  
- Enable IP routing.

### 3. IPs and gateways on PCs
- PC1: IP 192.168.10.x, mask 255.255.255.0, gateway 192.168.10.1.  
- PC2: IP 192.168.20.x, gateway 192.168.20.1.  
- PC3: IP 192.168.30.x, gateway 192.168.30.1.  
- PC4: IP 192.168.40.x, gateway 192.168.40.1.

### 4. Routing between L3 switches and router
- Connect each L3 switch to the router with a routed link (not access VLAN).  
- Give IPs to those links (e.g. Left L3–Router: 10.0.0.1/30 & 10.0.0.2/30; Right L3–Router: 10.0.0.5/30 & 10.0.0.6/30).  
- On each L3 switch, add default route to router (e.g. `ip route 0.0.0.0 0.0.0.0 10.0.0.2` etc.).  
- On router, add static routes back to each VLAN subnet via correct L3 switch IP (10.0.0.1 for VLAN 10/20 networks, 10.0.0.5 for VLAN 30/40 networks).

### 5. Test
- From PC1, ping gateway 192.168.10.1.  
- Ping 192.168.40.1 (SVI for VLAN 40).  
- Then ping PC4 IP 192.168.40.x.  

If all steps are correct, ping from PC1 (VLAN 10) to PC4 (VLAN 40) will succeed.


### 1. Left L3 switch → router

Goal: route VLAN 10 and 20 via router.

1) Make uplink a routed port and give IP:
```bash
conf t
interface g1/0/1
 no switchport
 ip address 10.0.0.1 255.255.255.252
 no shutdown
exit
ip routing
```
2) Add static default route to router:
```bash
ip route 0.0.0.0 0.0.0.0 10.0.0.2
end
wr
```

### 2. Right L3 switch → router

Goal: route VLAN 30 and 40 via router.

1) Make uplink a routed port and give IP:
```bash
conf t
interface g1/0/1
 no switchport
 ip address 10.0.0.5 255.255.255.252
 no shutdown
exit
ip routing
```
2) Add static default route to router:
```bash
ip route 0.0.0.0 0.0.0.0 10.0.0.6
end
wr
```

### 3. Router configuration

Goal: know how to reach all VLAN networks via each L3 switch.

Assume:
- VLAN 10: 192.168.10.0/24 via left L3  
- VLAN 20: 192.168.20.0/24 via left L3  
- VLAN 30: 192.168.30.0/24 via right L3  
- VLAN 40: 192.168.40.0/24 via right L3  

1) Configure both router interfaces:
```bash
conf t
interface g0/0
 ip address 10.0.0.2 255.255.255.252
 no shutdown
exit
interface g0/1
 ip address 10.0.0.6 255.255.255.252
 no shutdown
exit
```

2) Add static routes back to VLANs:
```bash
ip route 192.168.10.0 255.255.255.0 10.0.0.1
ip route 192.168.20.0 255.255.255.0 10.0.0.1
ip route 192.168.30.0 255.255.255.0 10.0.0.5
ip route 192.168.40.0 255.255.255.0 10.0.0.5
end
wr
```

### 4. Quick verification

On each L3 switch:
```bash
show ip route
ping 10.0.0.2      # from left L3
ping 10.0.0.6      # from right L3
```

Then from PC1:
- Ping 192.168.10.1 (its gateway)  
- Ping 192.168.40.1 (VLAN 40 SVI)  
- Ping PC4 IP  

If these work, routing between L3 switches and router is correct.




### Devices and VLANs

- Left side:  
  - PC1 in VLAN 10, PC2 in VLAN 20.  
  - These connect to a left L2 switch.  
  - Left L2 switch uplinks (trunk) to a left L3 switch.

- Right side:  
  - PC3 in VLAN 30, PC4 in VLAN 40.  
  - These connect to a right L2 switch.  
  - Right L2 switch uplinks (trunk) to a right L3 switch.

### L3 switches and router

- Left L3 switch:  
  - Has SVIs for VLAN 10 and 20 (gateway IPs for PC1 and PC2).  
  - Uplink to router is a routed port with IP 10.0.0.1/30.  
  - Default route: `ip route 0.0.0.0 0.0.0.0 10.0.0.2`.

- Right L3 switch:  
  - Has SVIs for VLAN 30 and 40 (gateway IPs for PC3 and PC4).  
  - Uplink to router is a routed port with IP 10.0.0.5/30.  
  - Default route: `ip route 0.0.0.0 0.0.0.0 10.0.0.6`.

- Router:  
  - Interface g0/0: 10.0.0.2/30 (to left L3).  
  - Interface g0/1: 10.0.0.6/30 (to right L3).  
  - Static routes:  
    - `ip route 192.168.10.0 255.255.255.0 10.0.0.1` (to VLAN 10, left).  
    - `ip route 192.168.20.0 255.255.255.0 10.0.0.1` (to VLAN 20, left).  
    - `ip route 192.168.30.0 255.255.255.0 10.0.0.5` (to VLAN 30, right).  
    - `ip route 192.168.40.0 255.255.255.0 10.0.0.5` (to VLAN 40, right).
https://chat.deepseek.com/share/d8ijh4h5mn72pe1j18
### What this achieves

- Each PC talks to its own gateway SVI on its local L3 switch.  
- Each L3 switch sends “non-local” traffic to the router (default route).  
- The router knows which L3 switch to use for each VLAN network (static routes).  
- Result: PC1 (VLAN 10) can ping PC4 (VLAN 40) through L3 switches and the router.


https://github.com/7elmie/CCNA-3-Lab-S-4-U/blob/main/IP%20Telephony%20completed.pkt
