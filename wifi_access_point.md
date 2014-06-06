# How to connect to my laptop using its wifi card

## How to have an Atheros wifi card acting as an access point on Debian

### Reference

[http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point](<http://www.cyberciti.biz/faq/debian-ubuntu-linux-setting-wireless-access-point>)

### Procedure

As root:

`apt-get install hostapd`

`sed -i '/^#DAEMON_CONF/a DAEMON_CONF="/etc/hostapd/hostapd.conf"' /etc/default/hostapd`

`cat << EOF | tee /etc/hostapd/hostapd.conf
bridge=br0
driver=ath5k
country_code=IT
ssid=doublem
hw_mode=g
channel=6

wpa=2
wpa_passphrase=MyWiFiPassword
## Key management algorithms ##
wpa_key_mgmt=WPA-PSK 
## Set cipher suites (encryption algorithms) ##
## TKIP = Temporal Key Integrity Protocol
## CCMP = AES in Counter mode with CBC-MAC
wpa_pairwise=TKIP
rsn_pairwise=CCMP
## Shared Key Authentication ##
auth_algs=1
## Accept all MAC address ###
macaddr_acl=0
EOF
`

Test (`-dd` means verbose debug mode):

`hostapd -dd /etc/hostapd/hostapd.conf
/etc/init.d/hostapd restart
`
