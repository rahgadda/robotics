# Raspberrian - PI 4

## Overview

## Installation
- Install Image Flasher from [here](https://www.raspberrypi.com/software/) and 64 bit OS.
- Configure router to always allocate static ip address to pi, I am using TPLink Router. 
  - Login to admin [here](http://192.168.0.1)
  - Navigate to `Advance --> Network --> DHCP Server --> Add Reservation`, Add Mac Adress and Static IP, i have used 192.168.0.5
- Enable `remote SSH` by navigating to `Home --> Preferences --> Raspberry Pi Configuration --> Interface --> SSH`
- Enable `remote VNC` by navigating to `Home --> Preferences --> Raspberry Pi Configuration --> Interface --> VNC`. This can also be enabled via ssh using `sudo raspi-config`
  - Check if service is running `systemctl status vncserver-x11-serviced`
  - Download TigerVNC from [here](https://www.realvnc.com/en/connect/download/viewer/)
  - Connect with details `192.168.0.5::5900`. Port details can be identified using command `netstat -tuln`
    ```bash
    root@raspberrypi:/home/kiaan/mydev# netstat -tuln
    Active Internet connections (only servers)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
    tcp        0      0 0.0.0.0:5900            0.0.0.0:*               LISTEN
    ```
- Buy any VR Controller [like](https://www.flipkart.com/umido-vr-controller/p/itmekhutjeapruqp) this and connect to PI using bluethooth.   
  ![](../01-images/VR_Controller_Connectivity.png)

<<<<<<< HEAD
  # List device details of root file system
  mount | grep root

  # Grow the partition size
  growpart /dev/mmcblk1 3 
  btrfs filesystem resize max /

  # List block storage & check if updated
  lsblk

  # List Partition
  cat /proc/partitions


  ## -- Install UI
  yum groupinstall "Server with GUI"
  systemctl set-default graphical
  reboot

  ## -- Enable Wifi
  ## Note: when wifi is enabled, it disables the eth0. You need to run “rmmod brcmfmac” to restore access via eth0

  # Install this if it is not installed
  # dnf -y install wpa_supplicant

  # Update with your NETWORK_SSID and NETWORK_PASSWORD/PSA
  cat << EOF > /etc/wpa_supplicant/wpa_supplicant.conf
  ctrl_interface=/var/run/wpa_supplicant
  ctrl_interface_group=wheel
  country=US
  p2p_disabled=1

  network={
  scan_ssid=1
  ssid="<NETWORK_SSID>"
  psk="<NETWORK_PASSWORD>"
  }
  EOF

  # Remove module brcmfmac
  rmmod brcmfmac

  # Add module brcmfmac
  modprobe -vvv brcmfmac

  # Create wlan0 service
  cat << EOF > /etc/systemd/system/network-wireless@.service
  [Unit]
  Description=Wireless network connectivity (%i)
  Wants=network.target
  Before=network.target
  BindsTo=sys-subsystem-net-devices-%i.device
  After=sys-subsystem-net-devices-%i.device

  [Service]
  Type=oneshot
  RemainAfterExit=yes

  ExecStart=modprobe -vvv -r brcmfmac
  ExecStart=modprobe -vvv brcmfmac
  ExecStart=/usr/sbin/ip link set dev %i up
  ExecStart=/usr/sbin/wpa_supplicant -B -i %i -c /etc/wpa_supplicant/wpa_supplicant.conf
  ExecStart=/usr/sbin/dhclient -v %i

  ExecStop=/usr/sbin/ip link set dev %i down

  [Install]
  WantedBy=multi-user.target
  EOF

  # Enable service network-wireless@.service
  ln -s /etc/systemd/system/network-wireless@.service /etc/systemd/system/multi-user.target.wants/network-wireless@wlan0.service
  
  systemctl daemon-reload
  systemctl start network-wireless@wlan0.service

  ## -- Enable Bluetooth
  yum install -y bluez
  
  # Enable Modules
  rmmod rfcomm btsdio btusb bnep bluetooth
  modprobe -vvv rfcomm btusb btsdio bnep

  # Enable Bluetooth Service
  systemctl enable bluetooth
  systemctl start bluetooth
  systemctl status bluetooth

  # Working with Bluetooth
  bluetoothctl devices
  bluetoothctl agents
  bluetoothctl defualt-agent
  bluetoothctl scan on
  bluetoothctl scan off
  ```


## References
- **Oracle Linux OS -** [Overview](https://docs.oracle.com/en/learn/oracle-linux-install-rpi/#prepare-the-installation-media), [Download](https://www.oracle.com/linux/downloads/linux-arm-downloads.html), [Article on Networking](https://community.ibm.com/community/user/cloud/blogs/alexei-karve/2022/11/27/microshift-27)
- **Balena Etcher** [Overview](https://etcher.balena.io/), [Installation](https://geraldonit.com/2019/08/11/how-to-install-oracle-linux-on-a-raspberry-pi-the-easy-way/)

=======
>>>>>>> febfacd96f7852e9653c10701fcadcb76073ac89