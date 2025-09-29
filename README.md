```

enable

configure terminal

Router(config)#interface Gig0/1

Router(config-if)#ip address 192.168.11.1 255.255.255.0

Router(config-if)#no shutdown

```

```

// configure static route(router to router , static hop)

ip route <destination-network> <subnet-mask> <next-hop-ip>

```





```

// 2 switch one router

Router(config)#interface Gig0/1

Router(config-if)#ip address 192.168.11.1 255.255.255.0

Router(config-if)#ip  helper-address 192.168.10.2 (dhcp server ip)

Router(config-if)#no shutdown



```

```

enable

configure terminal



! Step 1: Configure the router's interfaces (the gateways)

interface GigabitEthernet0/0

 ip address 192.168.1.1 255.255.255.0

 no shutdown

exit



interface GigabitEthernet0/1

 ip address 192.168.2.1 255.255.255.0

 no shutdown

exit



! Step 2: Exclude the gateway addresses from being assigned to PCs

ip dhcp excluded-address 192.168.1.1

ip dhcp excluded-address 192.168.2.1



! Step 3: Create the DHCP pool for the first network (LAN1)

ip dhcp pool LAN1

 network 192.168.1.0 255.255.255.0

 default-router 192.168.1.1

 dns-server 8.8.8.8

exit



! Step 4: Create the DHCP pool for the second network (LAN2)

ip dhcp pool LAN2

 network 192.168.2.0 255.255.255.0

 default-router 192.168.2.1

 dns-server 8.8.8.8

exit

```



```
configure terminal

! --- 1. Define Inside and Outside Interfaces ---
interface GigabitEthernet0/0
 ip nat inside
 exit
interface GigabitEthernet0/1
 ip nat inside
 exit
interface GigabitEthernet0/2
 ip nat inside
 exit
interface Serial0/0/0
 ip nat outside
 exit

! --- 2. Static NAT (for PC0 on 192.168.10.0 LAN) ---
! Translates 192.168.10.2 <--> 50.0.0.2
ip nat inside source static 192.168.10.2 50.0.0.2

! --- 3. Dynamic NAT (for 192.168.11.0 LAN) ---
! Defines the public IP address pool
ip nat pool DYNAMIC_POOL 60.0.0.0 60.0.0.5 netmask 255.0.0.0
! Identifies the internal network that is allowed to use the pool
access-list 1 permit 192.168.11.0 0.0.0.255
! Links the access list to the pool
ip nat inside source list 1 pool DYNAMIC_POOL

! --- 4. PAT (for 192.168.12.0 LAN) ---
! Create a pool with just one IP address
ip nat pool PAT_POOL 70.0.0.1 70.0.0.1 netmask 255.0.0.0

! Create the access list (same as before)
access-list 2 permit 192.168.12.0 0.0.0.255

! Link the ACL to the pool and add the 'overload' keyword
ip nat inside source list 2 pool PAT_POOL overload

end
```


