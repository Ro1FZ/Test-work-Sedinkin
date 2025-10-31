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
router2
```
[admin@RouterOS] > /interface vlan
[admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=10 name=VLAN1                                                                                                                              [admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=10 name=VLAN1                                                                                                                              0 comment="Client Network"
[admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=20 name=VLAN2                                                                                                                              [admin@RouterOS] /interface vlan> add interface=ether2 vlan-id=20 name=VLAN2                                                                                                                              0 comment="Server Network"
[admin@RouterOS] /interface vlan> /ip address
[admin@RouterOS] /ip address> add address=172.16.1.1/24 interface=VLAN10 com                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.1.1/24 interface=VLAN10 com                                                                                                                              ment="VLAN10 Gateway"
[admin@RouterOS] /ip address> add address=172.16.2.1/24 interface=VLAN20 com                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.2.1/24 interface=VLAN20 com                                                                                                                              ment="VLAN20 Gateway"
[admin@RouterOS] /ip address> add address=172.16.2.254/24 interface=ether1 c                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.2.254/24 interface=ether1 c                                                                                                                              omment="Syslog to Linux SERVER"
[admin@RouterOS] /ip address> /interface bridge
[admin@RouterOS] /interface bridge> add name=bridge-vlan10 comment="VLAN 10                                                                                                                               [admin@RouterOS] /interface bridge> add name=bridge-vlan10 comment="VLAN 10                                                                                                                               Bridge"
[admin@RouterOS] /interface bridge> add name=bridge-vlan20 comment="VLAN 20                                                                                                                               [admin@RouterOS] /interface bridge> add name=bridge-vlan20 comment="VLAN 20                                                                                                                               Bridge"
[admin@RouterOS] /interface bridge> /interface bridge port
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan10 interface=                                                                                                                              add bridge=bridge-vlan10 interface=VLAN10
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan10 interface=                                                                                                                              [admin@RouterOS] /interface bridge port> add bridge=bridge-vlan10 interface=                                                                                                                              ether3 comment="Linux Client 1"
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              add bridge=bridge-vlan20 interface=VLAN20
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              [admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              ether4 comment="Windows Server"
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              [admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              ether5 comment="Linux SERVER e1"
[admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              [admin@RouterOS] /interface bridge port> add bridge=bridge-vlan20 interface=                                                                                                                              ether1 comment="Linux SERVER e0 Syslog"
[admin@RouterOS] /interface bridge port> /ip address
[admin@RouterOS] /ip address> remove [find interface=VLAN10]
[admin@RouterOS] /ip address> remove [find interface=VLAN20]
[admin@RouterOS] /ip address> add address=172.16.1.1/24 interface=bridge-vla                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.1.1/24 interface=bridge-vla                                                                                                                              n10 comment="VLAN10 Gateway"
[admin@RouterOS] /ip address> add address=172.16.2.1/24 interface=bridge-vla                                                                                                                              [admin@RouterOS] /ip address> add address=172.16.2.1/24 interface=bridge-vla                                                                                                                              n20 comment="VLAN20 Gateway"
[admin@RouterOS] /ip address> /ip route
[admin@RouterOS] /ip route> add dst-address=0.0.0.0/0 gateway=172.16.2.254 c                                                                                                                              [admin@RouterOS] /ip route> add dst-address=0.0.0.0/0 gateway=172.16.2.254 c                                                                                                                              omment="Default via Router 1"
[admin@RouterOS] /ip route> /ip dns
[admin@RouterOS] /ip dns> set servers=172.16.2.2 allow-remote-requests=yes
[admin@RouterOS] /ip dns> /ip firewall filter
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept connec                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept connec                                                                                                                              tion-state=established,related comment="Allow established"
[admin@RouterOS] /ip firewall filter> add chain=forward action=accept commen                                                                                                                              [admin@RouterOS] /ip firewall filter> add chain=forward action=accept commen                                                                                                                              t="Allow all forwarding for lab"
[admin@RouterOS] /ip firewall filter> /system logging action
[admin@RouterOS] /system logging action> add name=remote target=remote remot                                                                                                                              [admin@RouterOS] /system logging action> add name=remote target=remote remot                                                                                                                              e=172.16.2.3 remote-port=514
failure: action already exists with such a name
[admin@RouterOS] /system logging action> add name=remote1 target=remote remo                                                                                                                              [admin@RouterOS] /system logging action> add name=remote1 target=remote remo                                                                                                                              te=172.16.2.3 remote-port=514
[admin@RouterOS] /system logging action> /system logging
[admin@RouterOS] /system logging> add action=remote1 topics=info,!debug
[admin@RouterOS] /system logging> add action=remote1 topics=warning
[admin@RouterOS] /system logging> add action=remote1 topics=error
[admin@RouterOS] /system logging> add action=remote1 topics=critical
[admin@RouterOS] /system logging> /interface print
Flags: D - dynamic, X - disabled, R - running, S - slave
 #     NAME                                TYPE       ACTUAL-MTU L2MTU  MAX-                                                                                                                              L2MTU MAC-ADDRESS
 0  RS ether1                              ether            1500                                                                                                                                                50:AD:44:00:09:00
 1  R  ether2                              ether            1500                                                                                                                                                50:AD:44:00:09:01
 2  RS ether3                              ether            1500                                                                                                                                                50:AD:44:00:09:02
 3  RS ether4                              ether            1500                                                                                                                                                50:AD:44:00:09:03
 4  RS ;;; Client Network
       VLAN10                              vlan             1500                                                                                                                                                50:AD:44:00:09:01
 5  RS ;;; Server Network
       VLAN20                              vlan             1500                                                                                                                                                50:AD:44:00:09:01
 6  R  ;;; VLAN 10 Bridge
       bridge-vlan10                       bridge           1500 65535                                                                                                                                          50:AD:44:00:09:01
 7  R  ;;; VLAN 20 Bridge
       bridge-vlan20                       bridge           1500 65535                                                                                                                                          50:AD:44:00:09:01
[admin@RouterOS] /system logging> /ip address print
Flags: X - disabled, I - invalid, D - dynamic
 #   ADDRESS            NETWORK         INTERFACE                                                                                                                                                         
 0   ;;; Syslog to Linux SERVER
     172.16.2.254/24    172.16.2.0      ether1                                                                                                                                                            
 1   ;;; VLAN10 Gateway
     172.16.1.1/24      172.16.1.0      bridge-vlan10                                                                                                                                                     
 2   ;;; VLAN20 Gateway
     172.16.2.1/24      172.16.2.0      bridge-vlan20                                                                                                                                                     
[admin@RouterOS] /system logging> /ip route print
Flags: X - disabled, A - active, D - dynamic, C - connect, S - static, r - rip, b - bgp, o - ospf, m - mme, B - b                                                                                         lackhole, U - unreachable, P - prohibit
 #      DST-ADDRESS        PREF-SRC        GATEWAY            DISTANCE
 0   S  ;;; Default via Router 1
        0.0.0.0/0                          172.16.2.254              1
 1 ADC  172.16.1.0/24      172.16.1.1      bridge-vlan10             0
 2 ADC  172.16.2.0/24      172.16.2.254    bridge-vlan20             0
[admin@RouterOS] /system logging> ping 172.16.2.254 count=5
bad command name ping (line 1 column 1)
[admin@RouterOS] /system logging> /ping 172.16.2.254 count=5
  SEQ HOST                                     SIZE TTL TIME  STATUS                                                                                                                                      
    0 172.16.2.254                               56  64 0ms
    1 172.16.2.254                               56  64 0ms
    2 172.16.2.254                               56  64 0ms
    3 172.16.2.254                               56  64 0ms
    4 172.16.2.254                               56  64 0ms
    sent=5 received=5 packet-loss=0% min-rtt=0ms avg-rtt=0ms max-rtt=0ms

[admin@RouterOS] /system logging> /user add name=oxidized password=Oxidized123! group=full comment="Oxidized Back                                                                                         [admin@RouterOS] /system logging> /user add name=oxidized password=Oxidized123! group=full comment="Oxidized Back                                                                                         up"
[admin@RouterOS] /system logging> /ip service set ssh address=172.16.2.0/24 port=22
[admin@RouterOS] /system logging> /user add name=oxidized password=Oxidized123! group=full comment="Oxidized Back                                                                                         [admin@RouterOS] /system logging> /user add name=oxidized password=Oxidized123! group=full comment="Oxidized Back                                                                                         up"
[admin@RouterOS] /system logging> /ip service set ssh address=172.16.2.0/24 port=22
[admin@RouterOS] /system logging>

```
![[Pasted image 20251031213208.png]]
