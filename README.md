- [unifi-openvpn](#unifi-openvpn)
  * [Generate the client/server/ca keys](#generate-the-client-server-ca-keys)
    + [CA](#ca)
    + [Server](#server)
    + [Client](#client)
    + [Generate Diffie Hellman](#generate-diffie-hellman)
    + [Copy the keys](#copy-the-keys)
- [Create config.gateway.json file in the controller](#create-configgatewayjson-file-in-the-controller)
- [Radius (Optional)](#radius--optional-)
- [Client](#client-1)
  * [Packages](#packages)
  * [Network Manager Settings](#network-manager-settings)
- [Useful links](#useful-links)

# unifi-openvpn
Steps how to configure openvpn in the Unifi

## Enable SSH auth
1) Enable in the controlle SSH authentication via Advanced Features
    - Controller -> Settings -> Site -> DEVICE AUTHENTICATION  
      [x] Enable SSH Authentication


2) Install easy-rsa in the SecurityGateway
```
$ ssh admin@SECURITY_GATEWAY_IP
$ sudo su -
# curl -O http://ftp.us.debian.org/debian/pool/main/e/easy-rsa/easy-rsa_2.2.2-1_all.deb
# sudo dpkg -i easy-rsa_2.2.2-1_all.deb
```

## Generate the client/server/ca keys
### CA
Common Name is "**OpenVPN CA**"
```
cd /usr/share/easy-rsa
. vars
./clean-all
./build-ca
```

### Server
Common Name is "**server**"
```
./build-key-server server
```

### Client
```
./build-key client
```
### Generate Diffie Hellman
```
./build-dh
```

### Copy the keys
```
mkdir /config/auth/keys/
cp keys/* /config/auth/keys/
```

# Create config.gateway.json file in the controller
$ cd /srv/unifi/data/sites/default  
$ vi [config.gateway.json](https://github.com/dougsland/unifi-openvpn/blob/main/CONTROLLER/srv/unifi/data/sites/default/config.gateway.json)

## Firewall

# Radius (Optional)

1) Enable Radius (Optional if you are using only auth keys)

   - Controller -> Settings -> Services -> Radius 

   - Server tab
     - Create secret
     - Authentication Port: 1812
     - AccountingPort: 1813
     - Account Interim Interval: 600
     - Tunnelled Reply: ON

   - Users tab
     - Name: YOUR_USERNAME
     - Password: YOUR_PASSWORD
     - Tunnel Type: 3- Layer Two Tunneling Protocol (L2TP)
     - Tunnel Medium Type: 1- IPv4 (IP version 4)


# Client
## Fedora 33

```
$ cat /etc/fedora-release 
Fedora release 33 (Thirty Three)
```
### Network Manager Settings

![](/png/NetworkManager/unifi_add_vpn_00.png)
![](/png/NetworkManager/unifi_add_vpn_01.png)
![](/png/NetworkManager/unifi_add_vpn_02.png)
![](/png/NetworkManager/unifi_add_vpn_03.png)
![](/png/NetworkManager/unifi_add_vpn_04.png)
![](/png/NetworkManager/unifi_add_vpn_05.png)
![](/png/NetworkManager/unifi_add_vpn_06.png)

### Packages
```
$ rpm -qa | grep NetworkManager
NetworkManager-l2tp-gnome-1.8.2-2.fc33.x86_64
NetworkManager-openvpn-gnome-1.8.12-1.fc33.1.x86_64
NetworkManager-ssh-1.2.11-2.fc33.x86_64
NetworkManager-vpnc-1.2.6-5.fc33.x86_64
NetworkManager-vpnc-gnome-1.2.6-5.fc33.x86_64
NetworkManager-ssh-gnome-1.2.11-2.fc33.x86_64
NetworkManager-openvpn-1.8.12-1.fc33.1.x86_64
NetworkManager-openconnect-gnome-1.2.6-5.fc33.x86_64
NetworkManager-strongswan-gnome-1.5.0-2.fc33.x86_64
NetworkManager-pptp-1.2.8-2.fc33.1.x86_64
NetworkManager-openconnect-1.2.6-5.fc33.x86_64
NetworkManager-l2tp-1.8.2-2.fc33.x86_64
NetworkManager-strongswan-1.5.0-2.fc33.x86_64
NetworkManager-pptp-gnome-1.2.8-2.fc33.1.x86_64
NetworkManager-libnm-1.26.4-1.fc33.x86_64
NetworkManager-1.26.4-1.fc33.x86_64
NetworkManager-wwan-1.26.4-1.fc33.x86_64
NetworkManager-bluetooth-1.26.4-1.fc33.x86_64
NetworkManager-adsl-1.26.4-1.fc33.x86_64
NetworkManager-ppp-1.26.4-1.fc33.x86_64
NetworkManager-team-1.26.4-1.fc33.x86_64
NetworkManager-wifi-1.26.4-1.fc33.x86_64
NetworkManager-config-connectivity-fedora-1.26.4-1.fc33.noarch
```


# Useful links
[UniFi - Accounts and Passwords for Controller, Cloud Key and Othe Devices](https://help.ui.com/hc/en-us/articles/204909374-UniFi-Accounts-and-Passwords-for-Controller-Cloud-Key-and-Other-Devices)  
https://blog.configwizard.xyz/configuring-openvpn-on-a-unifi-security-gateway/  
https://medium.com/server-guides/how-to-setup-an-openvpn-server-on-a-unifi-usg-e33ea2f6725d
