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


