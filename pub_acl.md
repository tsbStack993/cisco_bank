no access-list 120 permit udp any any eq bootps
no access-list 120 permit udp any any eq bootpc
no access-list 120 deny ip 192.168.20.0 0.0.0.255 192.168.0.0 0.0.255.255
no access-list 120 permit ip 192.168.20.0 0.0.0.255 any

!optional
interface g0/0.20
 no ip access-group 120 in

access-list 120 permit udp any any eq bootps
access-list 120 permit udp any any eq bootpc
access-list 120 permit ip 192.168.20.0 0.0.0.255 host 192.168.80.15
access-list 120 permit ip 192.168.20.0 0.0.0.255 host 10.0.0.6
access-list 120 deny ip 192.168.20.0 0.0.0.255 192.168.0.0 0.0.255.255
access-list 120 permit ip 192.168.20.0 0.0.0.255 any

interface g0/0.20
 ip access-group 120 in
