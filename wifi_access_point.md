# How to connect to my laptop using its wifi card

## How to have an Atheros wifi card acting as an access point on Debian

### Reference

[http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point](<http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point>)

### Procedure

As root:

```
apt-get install hostapd
update-rc.d -f hostap remove

sed -i '/^#DAEMON_CONF/a DAEMON_CONF="/etc/hostapd/hostapd.conf"' /etc/default/hostapd

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

Manually start the daemon
```
/etc/init.d/hostapd start
```

### Compatibility notes
