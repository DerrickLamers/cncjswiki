# [Wireless Setup](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=139866
https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=139486

```
# Bring Wireless Up
sudo ifup wlan0

# Scan for wireless networks
sudo iwlist wlan0 scan

# Open the wpa-supplicant configuration file in nano:
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
# ----------
network={
  ssid="YOUR_SSID"
  scan_ssid=1
  psk="YOUR_PASSKEY"
  mode=0
  proto=WPA2
  key_mgmt=WPA-PSK
  pairwise=CCMP
  group=CCMP
  auth_alg=OPEN
  id_str="raspi"
  priority=1
}
# ----------

# Restart Interface
sudo ifdown wlan0
sudo ifup wlan0

# Check Interface for IP
ifconfig wlan0
```