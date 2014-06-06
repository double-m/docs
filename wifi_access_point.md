### How to connect via SSH to my laptop using its Wifi card as an access point

Compatibility note: the built-in Atheros is working, the old PCMCIA DWL-610 is not
```
lspci -vv |grep -i -A 9 atheros
04:05.0 Ethernet controller: Atheros Communications Inc. AR2413 802.11bg NIC (rev 01)
        ...
        Kernel driver in use: ath5k
        Kernel modules: ath5k

lspci -vv |grep -i -A 9 d-link
05:00.0 Ethernet controller: D-Link System Inc DWL-510 2.4GHz Wireless PCI Adapter (rev 20)
        ...
        Kernel driver in use: rtl8180
        Kernel modules: rtl8180
```

References:

[http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point](<http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point>)

[http://blog.dmaggot.org/2010/05/setting-up-an-atheros-based-ap-using-ath5k-and-hostapd](<http://blog.dmaggot.org/2010/05/setting-up-an-atheros-based-ap-using-ath5k-and-hostapd>)

[http://askubuntu.com/questions/180733/how-to-setup-a-wi-fi-hotspot-access-point-mode](<http://askubuntu.com/questions/180733/how-to-setup-a-wi-fi-hotspot-access-point-mode>)

Installation and configuration of `hostapd` (as root):

```
apt-get install hostapd
update-rc.d -f hostap remove

# enable hostapd
sed -i '/^#DAEMON_CONF/a DAEMON_CONF="/etc/hostapd/hostapd.conf"' /etc/default/hostapd

# configure hostapd
cat << EOF | tee /etc/hostapd/hostapd.conf
interface=wlan0
driver=nl80211
country_code=IT
ssid=double-m
hw_mode=g
channel=1
auth_algs=1
wpa=2
wpa_passphrase=testtest
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
macaddr_acl=0
EOF

```

Test (`-dd` means verbose debug mode):

```
hostapd -dd /etc/hostapd/hostapd.conf

```
(now a wifi client can connect and authenticate, but it won't get any IP address, so let's stop the daemon with a CTRL-C)

Installation and configuration of `isc-dhcp-server` (as root):

```
# install the dhcpd
apt-get install isc-dhcp-server
update-rc.d -f isc-dhcp-server remove

# enable the dhcpd on wlan0
sed -i 's/""/"wlan0"/' /etc/default/isc-dhcp-server

# configure the dhcpd
mv /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.orig
cat << EOF | tee /etc/dhcp/dhcpd.conf
subnet 10.10.0.0 netmask 255.255.255.0 {
        range 10.10.0.2 10.10.0.16;
        option domain-name-servers 8.8.4.4, 208.67.222.222;
        option routers 10.10.0.1;
}
EOF

# edit /etc/network/interfaces so that
cat /etc/network/interfaces | grep -A 4 wlan0
	auto wlan0
	iface wlan0 inet static
	address 10.10.0.1
	netmask 255.255.255.0

# activate the new configuration for wlan0
ifconfig wlan0 down # or ifdown if it was already configured
ifup wlan0

```

Manually start the daemons:
```
/etc/init.d/hostapd start
/etc/init.d/isc-dhcp-server start

```

For the final test, connect to the laptop using a Wifi device having an SSH client installed on it:
```
ssh user@10.10.0.1

```

If needed at boot:
```
update-rc.d hostapd defaults
update-rc.d isc-dhcp-server defaults

```
