# Transparent-WiFi-AP-tutorial
Here is a tutorial to create a transparent/bridged AP with raspbian/debian/ubuntu distros:

*First we update everything:

        sudo apt-get update  
        sudo apt-get upgrade  

*Install hostapd and bridge utilities and we have to stop hostapd after it is installed:

        sudo apt-get install hostapd bridge-utils  
        sudo systemctl stop hostapd  

*Create bridge br0:

        sudo brctl addbr br0  

*Create a file /etc/hostapd/hostapd.conf 

        interface=wlan0  
        bridge=br0  
        #driver=nl80211  
        ssid=ADD-YOUR-SSID-HERE  
        hw_mode=g  
        channel=7  
        wmm_enabled=0  
        macaddr_acl=0  
        auth_algs=1  
        ignore_broadcast_ssid=0  
        wpa=2  
        wpa_passphrase=CreateYourPasswordHere123!!  
        wpa_key_mgmt=WPA-PSK  
        wpa_pairwise=TKIP  
        rsn_pairwise=CCMP  

                  **or if you're using a wireless n NIC (I have an ath9 USB NIC)**
          
        interface=wlan0  
        driver=nl80211  
        bridge=br0  
        ssid=ADD-YOUR-SSID-HERE  
        hw_mode=g  
        channel=7  
        ieee80211n=1  
        wmm_enabled=1  
        macaddr_acl=0  
        auth_algs=1  
        ignore_broadcast_ssid=0  
        wpa=2  
        wpa_passphrase=CreateYourPasswordHere123!!  
        wpa_key_mgmt=WPA-PSK  
        wpa_pairwise=TKIP  
        rsn_pairwise=CCMP  

*Edit /etc/default/hostapd and change this (be sure to uncomment it too): 

        DAEMON_CONF="/etc/hostapd/hostapd.conf"  

*Add eth0 to br0:

        sudo brctl addif br0 eth0  

*Edit /etc/network/interfaces and add this:

        auto br0  
        iface br0 inet dhcp  
        bridge_ports eth0 wlan0  

*Edit /etc/dhcpcd.conf and add the below at the top of the file (above the other interfaces):

        denyinterfaces eth0 wlan0  

*And finally reboot... you now have a bridged/transparent wireless AP.

**If you think you lost your AP, you can still connect to it via its hostname or IP, which you can obtain from your router/DHCP server by looking at at its leases**
