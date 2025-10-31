![[Pasted image 20251028202933.png]]
```
[admin@RouterOS] > /ip dhcp-client print
Flags: X - disabled, I - invalid, D - dynamic
 #   INTERFACE                                                                                                                                                                                                                                                    USE-PEER-DNS ADD-DEFAULT-R                                                                                                                        OUTE STATUS        ADDRESS
 0   ether1                                                                                                                                                                                                                                                       yes          yes                                                                                                                                       searching...
[admin@RouterOS] > /interface print
Flags: D - dynamic, X - disabled, R - running, S - slave
 #     NAME                                TYPE       ACTUAL-MTU L2MTU  MAX-L2MTU                                                                                                                         MAC-ADDRESS
 0  R  ether1                              ether            1500                                                                                                                                          50:3A:B6:00:08:00
 1  R  ether2                              ether            1500                                                                                                                                          50:3A:B6:00:08:01
 2  R  ether3                              ether            1500                                                                                                                                          50:3A:B6:00:08:02
 3  R  ether4                              ether            1500                                                                                                                                          50:3A:B6:00:08:03
[admin@RouterOS] > /interface vlan
[admin@RouterOS] /interface vlan> add interface=eth2 vlan-id=10 name=VLAN10 commen                                                                                                                        [admin@RouterOS] /interface vlan> add interface=eth2 vlan-id=10 name=VLAN10 commen                                                                                                                        t="Client Network Trunk"
input does not match any value of interface
[admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=10 name=VLAN10 comm                                                                                                                        [admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=10 name=VLAN10 comm                                                                                                                        ent="Client Network Trunk"
[admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=20 name=VLAN20 comm                                                                                                                        [admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=20 name=VLAN20 comm                                                                                                                        ent="Server Network Trunk"
[admin@RouterOS] /interface vlan> /ip address
[admin@RouterOS] /ip address> add address=172.16.2.254/24 interface=VLAN20 c                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.2.254/24 interface=VLAN20 c                                                                                                                              omment="Management"
[admin@RouterOS] /ip address> /ip firewall nat
[admin@RouterOS] /ip firewall nat> add chain=srcnat out-interface=ether1 act                                                                                                                              [admin@RouterOS] /ip firewall nat> add chain=srcnat out-interface=ether1 act                                                                                                                              ion=masquerade comment="NAT to Internet"
[admin@RouterOS] /ip firewall nat> /ip firewall filter
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept connec                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept connec                                                                                                                              tion-state=established,related comment="Allow established"
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              erface=VLAN10 out-interface=ether1 comment="VLAN10 to Internet"
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              erface=VLAN20 out-interface=ether1 comment="VLAN20 to Internet"
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              erface=VLAN10 out-interface=VLAN20 comment="VLAN10 to VLAN20"
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept in-int                                                                                                                              erface=VLAN20 out-interface=VLAN10 comment="VLAN20 to VLAN10"
[admin@RouterOS] /ip firewall filter> /ip dns
[admin@RouterOS] /ip dns> set servers=8.8.8.8,1.1.1.1 allow-remote-requests=                                                                                                                              [admin@RouterOS] /ip dns> set servers=8.8.8.8,1.1.1.1 allow-remote-requests=                                                                                                                              yes
[admin@RouterOS] /ip dns> /interface print
Flags: D - dynamic, X - disabled, R - running, S - slave
 #     NAME                                TYPE       ACTUAL-MTU L2MTU  MAX-                                                                                                                              L2MTU MAC-ADDRESS
 0  R  ether1                              ether            1500                                                                                                                                                50:3A:B6:00:08:00
 1  R  ether2                              ether            1500                                                                                                                                                50:3A:B6:00:08:01
 2  R  ether3                              ether            1500                                                                                                                                                50:3A:B6:00:08:02
 3  R  ether4                              ether            1500                                                                                                                                                50:3A:B6:00:08:03
 4  R  ;;; Client Network Trunk
       VLAN10                              vlan             1500                                                                                                                                                50:3A:B6:00:08:01
 5  R  ;;; Server Network Trunk
       VLAN20                              vlan             1500                                                                                                                                                50:3A:B6:00:08:01
[admin@RouterOS] /ip dns> /ip address print
Flags: X - disabled, I - invalid, D - dynamic
 #   ADDRESS            NETWORK         INTERFACE                                                                                                                                                                                                                                                                                                                                                                   
 0   ;;; Management
     172.16.2.254/24    172.16.2.0      VLAN20                                                                                                                                                                                                                                                                                                                                                                      
[admin@RouterOS] /ip dns> /ip address print
Flags: X - disabled, I - invalid, D - dynamic
 #   ADDRESS            NETWORK         INTERFACE                                                                                                                                                                                                                                                                                                                                                                   
 0   ;;; Management
     172.16.2.254/24    172.16.2.0      VLAN20                                                                                                                                                                                                                                                                                                                                                                      
[admin@RouterOS] /ip dns> /ip route print
Flags: X - disabled, A - active, D - dynamic, C - connect, S - static, r - r                                                                                                                              ip, b - bgp, o - ospf, m - mme, B - blackhole, U - unreachable, P - prohibit                                                                                                                              
 #      DST-ADDRESS        PREF-SRC        GATEWAY            DISTANCE
 0 ADC  172.16.2.0/24      172.16.2.254    VLAN20                    0
[admin@RouterOS] /ip dns> /user add name=oxidized password=Oxidized123! grou                                                                                                                              [admin@RouterOS] /ip dns> /user add name=oxidized password=Oxidized123! grou                                                                                                                             p=full comment="Oxidized Backup User"
[admin@RouterOS] /ip dns> ..
[admin@RouterOS] /ip> ..
[admin@RouterOS] > /ip service
[admin@RouterOS] /ip service> set ssh address=172.16.2.0/24 port=22
[admin@RouterOS] /ip service> set telnet disabled=yes
[admin@RouterOS] /ip service> /user print
Flags: X - disabled
 #   NAME                                                                                                                                                                                                        GROUP                                                                                                                                                                                                        ADDRESS            LAST-LOGGED-IN
 0   ;;; system default user
     admin                                                                                                                                                                                                       full                                                                                                                                                                                               
 1   ;;; Oxidized Backup User
     oxidized                                                                                                                                                                                                    full                                                                                                                                                                                               
[admin@RouterOS] /ip service> /ip service print
Flags: X - disabled, I - invalid
 #   NAME                                              PORT ADDRESS                                                                                                                                                                                                              CERTI                                                                                                                              FICATE
 0 XI telnet                                              23
 1   ftp                                                 21
 2   www                                                 80
 3   ssh                                                 22 172.16.2.0/24                                                                                                                                 
 4 XI www-ssl                                            443                                                                                                                                                                                                                      none                                                                                                                              
 5   api                                               8728
 6   winbox                                            8291
 7   api-ssl                                           8729                                                                                                                                                                                                                      none                                                                                                                               
[admin@RouterOS] /ip service>

```
