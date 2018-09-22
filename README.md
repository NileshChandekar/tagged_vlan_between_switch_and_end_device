# tagged_vlan_between_switch_and_end_device
![Image vlan](https://github.com/NileshChandekar/tagged_vlan_between_switch_and_end_device/blob/master/vlan.png)


# Router configuration :- 

## First interface 0/0 configuration 
    ~~~
    configure terminal
    interface FastEthernet 0/0
    ip address 192.168.122.2 255.255.255.0
    no shutdown
    exit
    exit
    wr
    ~~~

## Configure a default gateway:

    ~~~
    configure terminal
    ip route 0.0.0.0 0.0.0.0 192.168.122.1
    end
    wr
    ~~~


## Ping the router’s default gateway:


    ~~~
    ping 192.168.122.1

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.122.1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 4/12/16 ms



    ping 8.8.8.8

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 16/28/48 ms
    ~~~



## Ensure that the router is configured to use the correct DNS server:

    ~~~
    configure terminal
    ip domain-lookup
    ip name-server 8.8.8.8
    end
    wr
    ~~~



## Second interface 1/0 configuration 


    ~~~
    conf t 
    interface fastEthernet 1/0
    ip add 10.10.10.1 255.255.255.0
    no shut     
    exit
    exit
    wr
    ~~~


## Interface details 

    ~~~
    sh ip int br
    Interface                  IP-Address      OK? Method Status                Protocol
    FastEthernet0/0            192.168.122.2   YES manual up                    up      
    FastEthernet1/0            10.10.10.1      YES manual up                    up      
    ~~~


## Configure NAT  - for internet access


    ~~~
    conf t 
    interface fastEthernet 0/0
    ip nat outside 
    exit  
    interface fastEthernet 1/0
    ip nat inside 
    exit
    ip nat inside source list 1 interface fastEthernet 0/0 overload
    access-list 1 permit 10.0.0.0 0.255.255.255
    end
    wr
    ~~~


