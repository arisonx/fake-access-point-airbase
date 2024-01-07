# Ativar o encaminhamento de pacotes


``` bash
root@root$ echo 1 > /proc/sys/net/ipv4/ip_forward
root@root$ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```



# Ativar o modo de monitoramento - airmon-ng

``` bash
root@root$ airmon-ng check kill
root@root$ airmon-ng start wlan0
```

# Subir o fake Access-Point

``` bash
root@root$ airbase-ng -e fake-ap -c 6 wlan0mon
e : ap name
c : channel
"wlan0mon" : monitor interface network
```



# Configurar o DHCP

## run:
``` bash
root@root$ nano /etc/dhcp/dhcpd.conf
```

## update:

``` bash
option domain-name "example.org";
option domain-name-servers 8.8.8.8;
default-lease-time 600;
max-lease-time 7200;
ddns-update-style none;
authoritative;
log-facility local7;

# config
subnet 10.1.1.0 netmask 255.255.255.0 {
range 10.1.1.15 10.1.1.25;
option subnet-mask 255.255.255.0;
option domain-name-servers 8.8.8.8;
option routers 10.1.1.1;

}

```
# isc-dhcp-server config



##  install:

``` bash
root@root$ sudo apt-get install isc-dhcp-server
```

## config
```bash
nano /etc/default/isc-dhcp-server  
```



## set:

``` bash
# v4 interface need "at0"
INTERFACESv4="at0"
INTERFACESv6=""
```



## Subindo a interface at0
``` bash
root@root$ ifconfig at0 10.1.1.1 netmask 255.255.255.0 up
root@root$ ifconfig -a
```

## Conecte um dispositivo no AP ou espere um cliente conectar-se, após reinicie o servidor DHCP:


``` bash
root@root$ sudo systemctl restart isc-dhcp-server
```


e pronto, os pacotes serão encaminhados para eth0 e a ponte será estabelecida, assim 
o cliente conectado na rede poderá requisitar servidores.

