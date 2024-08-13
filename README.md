# SIM7600G-H-4G-hat

Starting documenting everything we can using SIM7600G-H-4G and raspbberypi-zero-W using:
 - ModemManager: managuing the LTE connection and assisted GPS
 - NetworkManager: managing the connection to the internet
    - prioritize wwan over wlan but keep wifi open for ssh communication durring debuging


## configure gsm connection using mmcli & nmcli
https://unix.stackexchange.com/questions/113975/configure-gsm-connection-using-nmcli/268285#268285
https://askubuntu.com/questions/740584/enabling-serial-network-devices-with-modemmanager/740585

### mmcli
useful commands:
```
#list available modems
mmcli -L

#print status of modem 0
mmcli -m 0

#get location status
mmcli -m 0 --location-status

#location options
#selection of modem must always be before location options flags, as follows:

mmcli -m 0

Location options:
  --location-status                                              Show status of location gathering.
  --location-get                                                 Get all available location information.
  --location-monitor                                             Monitor all available location information.
  --location-enable-3gpp                                         Enable 3GPP location gathering.
  --location-disable-3gpp                                        Disable 3GPP location gathering.
  --location-enable-agps-msa                                     Enable MSA A-GPS location gathering.
  --location-disable-agps-msa                                    Disable MSA A-GPS location gathering.
  --location-enable-agps-msb                                     Enable MSB A-GPS location gathering.
  --location-disable-agps-msb                                    Disable MSB A-GPS location gathering.
  --location-enable-gps-nmea                                     Enable NMEA-based GPS location gathering.
  --location-disable-gps-nmea                                    Disable NMEA-based GPS location gathering.
  --location-enable-gps-raw                                      Enable raw GPS location gathering.
  --location-disable-gps-raw                                     Disable raw GPS location gathering.
  --location-enable-cdma-bs                                      Enable CDMA base station location gathering.
  --location-disable-cdma-bs                                     Disable CDMA base station location gathering.
  --location-enable-gps-unmanaged                                Enable unmanaged GPS location gathering.
  --location-disable-gps-unmanaged                               Disable unmanaged GPS location gathering.
  --location-set-supl-server=[IP:PORT] or [FQDN:PORT]            Set SUPL server address
  --location-inject-assistance-data=[PATH]                       Inject assistance data in the GNSS module
  --location-set-gps-refresh-rate=[RATE]                         Set GPS refresh rate in seconds, or 0 disable the explicit rate.
  --location-set-enable-signal                                   Enable location update signaling in DBus property.
  --location-set-disable-signal                                  Disable location update signaling in DBus property.

#restart ModemManager
systemctl restart ModemManager
```

### nmcli
start new connection named wwan:
```
nmcli c add type gsm ifname '*' con-name wwan apn internet

#set dns by
nmcli con mod <CONNECTION_NAME> ipv4.dns "8.8.8.8 8.8.4.4"

#ignore auto dns from the ISP by
nmcli con mod <CONNECTION_NAME> ipv4.ignore-auto-dns yes
nmcli con mod <CONNECTION_NAME> ipv4.route-metric 500
nmcli con mod <CONNECTION_NAME> connection.interface-name cdc-wdm0
#don't forget to restart
service NetworkManager restart
```

useful commands:
```
nmcli dev status
nmcli con show
nmcli connection show --active
nmcli connection delete <CONNECTION_NAME>

nmcli con edit <CONNECTION_NAME>
print
nmcli> set connection.interface-name cdc-wdm0
save
quit
```

# Useful links

## SIM7600G-H 4G HAT (B) wiki
https://www.waveshare.com/wiki/Main_Page
https://www.waveshare.com/wiki/SIM7600G-H_4G_HAT_(B)
https://www.waveshare.com/wiki/How_to_set_up_Raspberry_Pi_Zero_W_as_a_3G_4G_router

## Basic configuration PPP
https://tldp.org/HOWTO/PPP-HOWTO/index.html
https://hub.libre.computer/t/howto-configuring-a-simcom-sim7600g-or-sim7600x-hat-with-cellular-data-internet-connection/2333
https://www.twilio.com/docs/iot/supersim/getting-started-super-sim-raspberry-pi-waveshare-4g-hat

## Setting up the connection on raspbian
https://forums.raspberrypi.com/viewtopic.php?t=224355&start=100

## Speed test cli
https://askubuntu.com/questions/104755/how-to-check-internet-speed-via-terminal

## NMEA Analyser
https://swairlearn.bluecover.pt/nmea_analyser

## Open Database of Cell Towers
https://opencellid.org

## Composite USB Gadgets on the Raspberry Pi Zero
https://hackaday.com/2016/02/25/giving-the-pi-zero-usb-ethernet-and-serial-over-usb/
https://www.isticktoit.net/?p=1383