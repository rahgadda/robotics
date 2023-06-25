# Oracle Linux - PI 4

## Overview
- 

## Installtion
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

## Python
- Configuration listed below
  ```bash
  # Install dependencies
  sudo apt install libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev libtk8.6 libgdm-dev libdb4o-cil-dev libpcap-dev

  # Download Latest Python
  wget https://www.python.org/ftp/python/3.11.4/Python-3.11.4.tgz
  tar -zxvf Python-3.11.4.tgz
  cd  Python-3.11.4
  ./configure --enable-optimizations
  make altinstall
  
  wget https://bootstrap.pypa.io/get-pip.py | python3.11
  pip3.11 -V

  cd ..
  rm -rf Python-3.11.4.tgz

  # Install Virtual Env Library
  pip3.11 install virtualenv

  # Create a Project Folder
  mkdir -m 777 bluetooth
  cd bluetooth

  # Create virtual environment
  python-3.11 -m venv env
  source /home/kiaan/mydev/bluetooth/env/bin/activate
  python-3.11 --version
  pip3.11 install --upgrade pip
  pip3.11 freeze | xargs pip3.11 install --upgrade
  pip3.11 install -r requirements.txt
  deactivate
  ```