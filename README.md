# OpenVPN server setup on Linux and client on Windows 7 uses openvpngui

## Server
### Install openvpn server and easy-rsa
```bash
$ apt-get update
$ apt-get install openvpn easy-rsa
```

### Setup configuration
```bash
$ cd /etc/openpvn/easy-rsa

$ ./clean-all

$ ./build-ca

$ ./build-key-server vpn-server

$ ./build-key desktop-client

$ ./build-dh # may take long time, please wait.
```

### Create server.conf
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
```

### Restart openvpn server
```bash
service openvpn restart
```

### Check service
1.Check the process
2.1194 port on the netstat list.
```bash
$ ps aux | grep openvpn

$ netstat -tunlp | grep 1194
```

## Install OpenVPNGUI on windows7
### Download and install
https://openvpn.net/index.php/open-source/downloads.html

### Configuration file
Add a new configuration file at C:\Program Files\OpenVPN\config\client.ovpn

```txt
## client.ovpn ##
client
dev tap
proto udp
remote 192.168.10.10 1194 # Your vpn server's domain or ip address
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert desktop-client.crt
key desktop-client.key
comp-lzo
verb 3
route-method exe
route-delay 2
```

### Copy certificate files from openvpn server
Copy these files from openvpn server then put at C:\Program Files\OpenVPN\config\
* desktop-client.crt
* desktop-client.key
* ca.crt

### Startup OpenVPNGUI
![openvpn_gui_location.PNG](https://github.com/pinatclimax/install_openvpn/blob/master/imgs/openvpn_gui_location.PNG)

![openvpn_gui_small_icon.PNG](https://github.com/pinatclimax/install_openvpn/blob/master/imgs/openvpn_gui_small_icon.PNG)

### Check your ip
We can see the ip address has changed.

![ifconfig](https://github.com/pinatclimax/install_openvpn/blob/master/imgs/openvpn_gui_ifconfig.PNG)
