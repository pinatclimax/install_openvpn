# OpenVPN setup on Linux

## Install openvpn and easy-rsa
```bash
$ apt-get update
$ apt-get install openvpn easy-rsa
```

## 


## Create server.conf
```bash
$ vim /etc/openvpn/server.conf
```
```txt
port 1194
proto udp
dev tap
tun-mtu 1500
tun-mtu-extra 32
mssfix 1450
ca /etc/openvpn/easy-rsa/keys/ca.crt
cert /etc/openvpn/easy-rsa/keys/vpn-server.crt
key /etc/openvpn/easy-rsa/keys/vpn-server.key
dh /etc/openvpn/easy-rsa/keys/dh2048.pem
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
keepalive 5 30
comp-lzo
persist-key
persist-tun
status /var/log/openvpn.log
verb 3
```txt


