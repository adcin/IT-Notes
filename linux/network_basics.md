# Private IPv4 Address Ranges

- IANA: [IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)

Address ranges assigned by the IANA (Internet Assigned Numbers Authority) to be used by private networks:

- Class A: 10.0.0.0 to 10.255.255.255
- Class B: 172.16.0.0 to 172.31.255.255
- Class C: 192.168.0.0 to 192.168.255.255


</br>

# Subnet and IP ranges calculation

## sipcalc

command to make my life a little easier when determining IP address ranges, netmasks, and potential gateway addresses

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