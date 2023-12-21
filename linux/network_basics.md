# Private IPv4 Address Ranges

- IANA: [IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)

Address ranges assigned by the IANA (Internet Assigned Numbers Authority) to be used by private networks:

- Class A: 10.0.0.0 to 10.255.255.255
- Class B: 172.16.0.0 to 172.31.255.255
- Class C: 192.168.0.0 to 192.168.255.255


</br>

# Subnet and IP ranges calculation

## sipcalc

Command line tool for determining IP address ranges, netmasks, and potential gateway addresses.

- [routemeister.net - project website](http://www.routemeister.net/projects/sipcalc/index.html)
- [routemeister.net - manpage](http://www.routemeister.net/projects/sipcalc/sipcalc-man.html)

### sipcalc basic usage

</br>

IPv4 example:
```shell
$ sipcalc 192.168.0.0/24
-[ipv4 : 192.168.0.0/24] - 0

[CIDR]
Host address            - 192.168.0.0
Host address (decimal)  - 3232235520
Host address (hex)      - C0A80000
Network address         - 192.168.0.0
Network mask            - 255.255.255.0
Network mask (bits)     - 24
Network mask (hex)      - FFFFFF00
Broadcast address       - 192.168.0.255
Cisco wildcard          - 0.0.0.255
Addresses in network    - 256
Network range           - 192.168.0.0 - 192.168.0.255
Usable range            - 192.168.0.1 - 192.168.0.254
```

</br>

IPv4 example with -a (all) switch: 
```shell
$ sipcalc -a 192.168.0.0/24
-[ipv4 : 192.168.0.0/24] - 0

[Classful]
Host address            - 192.168.0.0
Host address (decimal)  - 3232235520
Host address (hex)      - C0A80000
Network address         - 192.168.0.0
Network class           - C
Network mask            - 255.255.255.0
Network mask (hex)      - FFFFFF00
Broadcast address       - 192.168.0.255

[CIDR]
Host address            - 192.168.0.0
Host address (decimal)  - 3232235520
Host address (hex)      - C0A80000
Network address         - 192.168.0.0
Network mask            - 255.255.255.0
Network mask (bits)     - 24
Network mask (hex)      - FFFFFF00
Broadcast address       - 192.168.0.255
Cisco wildcard          - 0.0.0.255
Addresses in network    - 256
Network range           - 192.168.0.0 - 192.168.0.255
Usable range            - 192.168.0.1 - 192.168.0.254

[Classful bitmaps]
Network address         - 11000000.10101000.00000000.00000000
Network mask            - 11111111.11111111.11111111.00000000

[CIDR bitmaps]
Host address            - 11000000.10101000.00000000.00000000
Network address         - 11000000.10101000.00000000.00000000
Network mask            - 11111111.11111111.11111111.00000000
Broadcast address       - 11000000.10101000.00000000.11111111
Cisco wildcard          - 00000000.00000000.00000000.11111111
Network range           - 11000000.10101000.00000000.00000000 -
                          11000000.10101000.00000000.11111111
Usable range            - 11000000.10101000.00000000.00000001 -
                          11000000.10101000.00000000.11111110

[Networks]
Network                 - 192.168.0.0     - 192.168.0.255 (current)
```

</br>

IPv6 example:
```shell
$ sipcalc 2001:0002::/48
-[ipv6 : 2001:0002::/48] - 0

[IPV6 INFO]
Expanded Address        - 2001:0002:0000:0000:0000:0000:0000:0000
Compressed address      - 2001:2::
Subnet prefix (masked)  - 2001:2:0:0:0:0:0:0/48
Address ID (masked)     - 0:0:0:0:0:0:0:0/48
Prefix address          - ffff:ffff:ffff:0:0:0:0:0
Prefix length           - 48
Address type            - Aggregatable Global Unicast Addresses
Network range           - 2001:0002:0000:0000:0000:0000:0000:0000 -
                          2001:0002:0000:ffff:ffff:ffff:ffff:ffff
```

</br>

Splitting a /24 network into /27 networks:  
```shell
$ sipcalc -s /27 192.168.0.0/24
-[ipv4 : 192.168.0.0/24] - 0

[Split network]
Network                 - 192.168.0.0     - 192.168.0.31
Network                 - 192.168.0.32    - 192.168.0.63
Network                 - 192.168.0.64    - 192.168.0.95
Network                 - 192.168.0.96    - 192.168.0.127
Network                 - 192.168.0.128   - 192.168.0.159
Network                 - 192.168.0.160   - 192.168.0.191
Network                 - 192.168.0.192   - 192.168.0.223
Network                 - 192.168.0.224   - 192.168.0.255
```

</br>

Interface specific calculation: 
```shell
$ sipcalc eth0
-[int-ipv4 : eth0] - 0

[CIDR]
Host address            - 192.168.131.208
Host address (decimal)  - 3232269264
Host address (hex)      - C0A883D0
Network address         - 192.168.131.0
Network mask            - 255.255.255.0
Network mask (bits)     - 24
Network mask (hex)      - FFFFFF00
Broadcast address       - 192.168.131.255
Cisco wildcard          - 0.0.0.255
Addresses in network    - 256
Network range           - 192.168.131.0 - 192.168.131.255
Usable range            - 192.168.131.1 - 192.168.131.254
```

</br>

## ipcalc

Also a command line tool for calculating IP ranges and subnets, but more focused on teaching networking basics.

### Description (from `man page`):
> ipcalc  takes  an  IPv4  address and netmask and calculates the resulting broadcast, network, Cisco wildcard mask, and host range. By giving a second netmask, you can design sub‐ and supernetworks. It is also intended to be a teaching tool and presents the results as easy‐to‐understand binary values.

</br>

IPv4 basic info (same as `ipcalc 192.168.0.0/24`):
```shell
ipcalc 192.168.0.0
#or
ipcalc 192.168.0.0/24
#or
ipcalc 192.168.0.0/255.255.255.0

```

```shell
Address:   192.168.0.0          11000000.10101000.00000000. 00000000
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   192.168.0.0/24       11000000.10101000.00000000. 00000000
HostMin:   192.168.0.1          11000000.10101000.00000000. 00000001
HostMax:   192.168.0.254        11000000.10101000.00000000. 11111110
Broadcast: 192.168.0.255        11000000.10101000.00000000. 11111111
Hosts/Net: 254                   Class C, Private Internet
```

</br>

Calculate one subnet with 10 hosts:
```shell
ipcalc 192.168.0.0 -s 10
```

```shell
Address:   192.168.0.0          11000000.10101000.00000000. 00000000
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   192.168.0.0/24       11000000.10101000.00000000. 00000000
HostMin:   192.168.0.1          11000000.10101000.00000000. 00000001
HostMax:   192.168.0.254        11000000.10101000.00000000. 11111110
Broadcast: 192.168.0.255        11000000.10101000.00000000. 11111111
Hosts/Net: 254                   Class C, Private Internet

1. Requested size: 10 hosts
Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
Network:   192.168.0.0/29       11000000.10101000.00000000.00000 000
HostMin:   192.168.0.1          11000000.10101000.00000000.00000 001
HostMax:   192.168.0.6          11000000.10101000.00000000.00000 110
Broadcast: 192.168.0.7          11000000.10101000.00000000.00000 111
Hosts/Net: 6                     Class C, Private Internet

Needed size:  16 addresses.
Used network: 192.168.0.0/28
Unused:
192.168.0.16/28
192.168.0.32/27
192.168.0.64/26
192.168.0.128/25
```

</br>

IPv4 info for a /27 network:
```shell
ipcalc 192.168.0.0/27
# or
ipcalc 192.168.0.0/255.255.255.224
```

```shell
Address:   192.168.0.0          11000000.10101000.00000000.000 00000
Netmask:   255.255.255.224 = 27 11111111.11111111.11111111.111 00000
Wildcard:  0.0.0.31             00000000.00000000.00000000.000 11111
=>
Network:   192.168.0.0/27       11000000.10101000.00000000.000 00000
HostMin:   192.168.0.1          11000000.10101000.00000000.000 00001
HostMax:   192.168.0.30         11000000.10101000.00000000.000 11110
Broadcast: 192.168.0.31         11000000.10101000.00000000.000 11111
Hosts/Net: 30                    Class C, Private Internet
```

-------------

</br>

# TCP and UDP ports

</br>

## Port ranges

|     Ports     | Description                         |
|:-------------:| ----------------------------------- |
|   0 - 1023    | Well-known ports or system ports    |
| 1024 - 49151  | Registered ports                    |
| 49152 – 65535 | Dynamic, private or ephemeral ports | 

</br>

## List of officially registered ports

- [iana.org - Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)
- [wikipedia.org - List of TCP and UDP port numbers](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)

```shell
cat /etc/services
```

-------------

</br>

# List open, listening ports of local machine

</br>

## ss

```shell
sudo ss -tulpn
```

</br>

## nmap

```shell
sudo nmap -sTU -O localhost
# alternative
sudo nmap -sTU -O 127.0.0.1
```

</br>

## lsof

```shell
sudo lsof -i -P -n | grep LISTEN
```

</br>

## netstat

```shell
sudo netstat -tulpn | grep LISTEN
```

</br>

## nc - netcat

```shell
nc -z -v 127.0.0.1 1-65535 2>&1 | grep -v 'Connection refused'
```