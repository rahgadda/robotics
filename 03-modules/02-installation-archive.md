# Oracle Linux - PI 4

## Overview
- 

## Installtion
- Below are the steps to install Oracle Linux 9 on Raspberry Pi
  ```bash
  ## -- Grow root folder to take all space
  # List block storage
  lsblk

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
  ```

## References
- **Oracle Linux OS -** [Overview](https://docs.oracle.com/en/learn/oracle-linux-install-rpi/#prepare-the-installation-media), [Download](https://www.oracle.com/linux/downloads/linux-arm-downloads.html), [Article on Networking](https://community.ibm.com/community/user/cloud/blogs/alexei-karve/2022/11/27/microshift-27)
- **Balena Etcher** [Overview](https://etcher.balena.io/), [Installation](https://geraldonit.com/2019/08/11/how-to-install-oracle-linux-on-a-raspberry-pi-the-easy-way/)
