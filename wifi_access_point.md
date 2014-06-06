### How to connect to my laptop using its wifi card
How to have an Atheros wifi card acting as an access point on Debian

References:

[http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point](<http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point>)

[http://blog.dmaggot.org/2010/05/setting-up-an-atheros-based-ap-using-ath5k-and-hostapd](<http://blog.dmaggot.org/2010/05/setting-up-an-atheros-based-ap-using-ath5k-and-hostapd>)

[http://askubuntu.com/questions/180733/how-to-setup-a-wi-fi-hotspot-access-point-mode](<http://askubuntu.com/questions/180733/how-to-setup-a-wi-fi-hotspot-access-point-mode>)

Configuration (as root):

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

Test (`-dd` means verbose debug mode, CTRL-C to stop the daemon):

```
hostapd -dd /etc/hostapd/hostapd.conf
```
(now a wifi client can connect and authenticate, but it won't get any IP address)

# TODO explain dhcpd

Manually start the daemons:
```
/etc/init.d/hostapd start
/etc/init.d/dhcpdd start

```

Compatibility notes: Atheros is working, DWL-610 is not
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

