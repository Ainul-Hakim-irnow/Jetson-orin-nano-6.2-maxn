# Jetson-orin-nano-6.2-maxn
For Seeed Studio reComputer **DO NOT** use ```sudo apt upgarde```

## Table of Content
1. [Install Python 3.10 and enviroment](#Install-Python-3.10-and-enviroment)
2. [Install pip for python 3.10](#-Install-pip)

## Install Python 3.10 and enviroment
1. Check if python 3.10 install
```
python3.10 --version
```
2. If no python 3.10 install, visit [github](https://github.com/Ainul-Hakim-irnow/Jetson-orin-nano-5.1.3.git)

## Install pip
```
sudo apt install nano
```
1. Open bashrc
```
nano ~/.bashrc
```
2. Scroll to the bottom and paste;
```
export CUDA_HOME=/usr/local/cuda
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64
export PATH=$PATH:$CUDA_HOME/bin
```
3. Press ```Ctrl + X```, then ```Y``` to save, then ```Enter``` to exit
4. Apply change
```
source ~/.bashrc
```
5. Install pip
```
sudo apt install python3-pip
```

## Install Jtop
1. Install Jtop
```
sudo python3.10 -m pip install -U jetson-stats
```
2. Initialize service
```
sudo systemctl restart jtop.service
```
3. Reboot
```
sudo reboot
```
4. After reboot, check Jtop
```
jtop
```

## Install VS Code
1. You can download VS Code [here](https://code.visualstudio.com/docs/?dv=linuxarm64_deb)

2. Open terminal
```
cd ~/Downloads
```
3. Install VS Code *.deb file
```
sudo dpkg -i package_name.deb
```

## Install Pylon
1. You can download *.tar.gz file [here](https://drive.google.com/file/d/1AKmBRzHc4yT-R1AkfoSfUR2PYj0U7Uz_/view?usp=sharing)

2. Open terminal
```
cd ~/Downloads
```
3. Unzip the file
```
sudo tar -xvf pylon_package_name.tar.gz
```
4. Install dependencies
```
sudo apt-get install libxcb-cursor0
```
5. Install pylon software
```
sudo dpkg -i pylon_package_name.deb
```
6. Install codemeter runtime
```
sudo dpkg -i codemeter_package_name.deb
```
7. Setup USB
```
sudo /opt/pylon/share/pylon/setup-usb.sh
```
8. Check Pylon Software. If success install pypylon library
```
pip install pypylon
```
10. Download OpenCV
```
pip install opencv-python
```
9. After successfull install pylon. Try it using [this](opencv.py) code above

## Install Web App
1. Open terminal and Install Epiphany Browser
```
sudo apt install epiphany-browser
```
2. Press ```Y``` to confrim and ENTER.
3. Set folder permission
```
sudo chown $USER:$USER $HOME/Downloads && chmod 755 $HOME/Downloads
```
4. Restart the web browser

## Install DWAgent
1. Visit the website [DWAgent](https://www.dwservice.net/en/home.html)
2. Open Terminal and navigate to Downloads folder
```
cd Downloads
```
3. Execute the permission
```
chmod +x dwagent.sh
```
4. Install the software
```
sudo ./dwagent.sh
```

## Remote Headless
1. Open Terminal and Download Xorg
```
sudo apt-get install xserver-xorg-video-dummy
```
2. Open Xorg config file
```
sudo nano /etc/X11/xorg.conf
```
3. Replace this text
```
# Copyright (c) 2011-2013 NVIDIA CORPORATION.  All Rights Reserved.

#
# This is the minimal configuration necessary to use the Tegra driver.
# Please refer to the xorg.conf man page for more configuration
# options provided by the X server, including display-related options
# provided by RandR 1.2 and higher.

# Disable extensions not useful on Tegra.
Section "Module"
    Disable     "dri"
    SubSection  "extmod"
        Option  "omit xfree86-dga"
    EndSubSection
EndSection

Section "Device"
    Identifier  "Tegra0"
    Driver      "nvidia"
# Allow X server to be started even if no display devices are connected.
    Option      "AllowEmptyInitialConfiguration" "true"
EndSection
```
To this text
```
Section "Device"
Identifier "Configured Video Device"
Driver "dummy"
# Default is 4MiB, this sets it to 16MiB
VideoRam 16384
EndSection

Section "Monitor"
Identifier "Configured Monitor"
HorizSync 31.5-48.5
VertRefresh 50-70
EndSection

Section "Screen"
Identifier "Default Screen"
Monitor "Configured Monitor"
Device "Configured Video Device"
DefaultDepth 24
SubSection "Display"
Depth 24
Modes "1920x1080"
EndSubSection
EndSection
```
4. Reboot jetson
```
sudo reboot
```
