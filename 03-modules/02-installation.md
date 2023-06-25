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
