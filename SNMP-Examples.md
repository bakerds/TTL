# SNMP Configuration Examples

## Linux
### Install snmpd
Ubuntu
```
apt-get install -y snmpd
```

CentOS
```
yum -y install net-snmp net-snmp-utils
```

For optional Linux distro detection, run:
```
curl -o /usr/bin/distro https://raw.githubusercontent.com/librenms/librenms-agent/master/snmp/distro
chmod +x /usr/bin/distro
```

### Configure snmpd
In `/etc/snmp/snmpd.conf`:
```
rocommunity  public 10.20.0.0/24
rocommunity6 public 2620:1d5:10:20::/64
sysLocation  Dan's VMware Host
sysContact   Dan
extend .1.3.6.1.4.1.2021.7890.1 distro /usr/bin/distro
extend .1.3.6.1.4.1.2021.7890.2 hardware '/bin/cat /sys/devices/virtual/dmi/id/product_name'
extend .1.3.6.1.4.1.2021.7890.3 manufacturer '/bin/cat /sys/devices/virtual/dmi/id/sys_vendor'
```


## VMware ESXi
```
esxcli system snmp set --communities public
esxcli system snmp set --enable true
```


## Cisco iOS SNMPv3
```
ip access-list standard SNMP
 permit host 10.20.0.20
 deny any log
exit

snmp-server location Location_1
snmp-server contact Dan

snmp-server group READONLY v3 priv
snmp-server user public READONLY v3 auth sha MyAuthPassword priv aes 128 MyPrivPassword access SNMP
```


## Cisco ASA SNMPv3
```
snmp-server location Location_1
snmp-server contact Dan

snmp-server group READONLY v3 priv
snmp-server user public READONLY v3 auth sha MyAuthPassword priv aes 128 MyPrivPassword

snmp-server host inside 10.20.0.20 version 3 public
```


## Ruckus ICX SNMPv3
```
ip access-list standard 1
 permit 10.20.0.20
 deny   any log
exit

snmp-server group READONLY v3 priv read all
snmp-server user public READONLY v3 access 1 auth sha MyAuthPassword priv aes MyPrivPassword

snmp-server location Location_1
snmp-server contact Dan
```


## Ruckus ICX SNMPv2
```
ip access-list standard 1
 permit 10.20.0.20
 deny   any log
exit

snmp-server community public ro 1

snmp-server location Location_1
snmp-server contact Dan
```


## Juniper SNMPv2
```
set snmp location "Location_1"
set snmp contact "Dan"
set snmp community public authorization read-only
set snmp community public clients 10.20.0.0/24
set snmp community public clients 2620:1d5:10:20::/64
```
