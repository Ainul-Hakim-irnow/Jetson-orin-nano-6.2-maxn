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

## Install Jetson Pytorch
1. Uninstall current Pytorch
```
pip3 uninstall torch torchvision torchaudio
```
2. Install Jetson's compatible Pytorch
```
pip3 install torch torchvision torchaudio --index-url https://pypi.jetson-ai-lab.io/jp6/cu126/
```
3. Downgrade Numpy2.xx to Numpy1.xx
```
pip3 install "numpy<2"
```
4. Verify Pytorch installation
```
python3 -c "import torch; print(f'PyTorch: {torch.__version__}'); print(f'CUDA Available: {torch.cuda.is_available()}'); print(f'CUDA Version: {torch.version.cuda}')"
```
5. If terminal pops up ImportError: libcudss.so.0 not found
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/arm64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cudss
```
6. Retry verification Pytorch installation

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
4. To remove keyring in vscode, Open VSCode. press ```Ctrl + Shift + P```. Type ```configure runtime``` and click to open ```argv.json``. Paste:
```
"password-store": "basic"
```
```
{
    "...",
    "...",
    "password-store": "basic"
}
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
9. Download OpenCV
```
pip install opencv-python
```
10. After successfull install pylon. Try it using [this](opencv.py) code above
11. If have error:  QFontDatabase: Cannot find font directory /home/ainul/anaconda3/envs/aintect/lib/python3.10/site-packages/cv2/qt/fonts.

Note that Qt no longer ships fonts. Deploy some (from https://dejavu-fonts.github.io/ for example) or switch to fontconfig. 
```
sudo apt-get install libfontconfig1 fonts-dejavu
```
12. Build link default font with cv2
```
ln -s /usr/share/fonts /home/ainul/anaconda3/envs/aintect/lib/python3.10/site-packages/cv2/qt/fonts
```

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

## Remote Headless (Important: If you set up xorg, you wont see the dislay using hdmi. make sure you can access it with ssh or other remote software!)
1. Open Terminal and Download Xorg
```
sudo apt-get install xserver-xorg-video-dummy
```
1.1. If failed ```E: Unable to locate package xserver-xorg-video-dummy```. Back up the sources.list
```
sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
```

1.2. Open ```sources.list```
```
sudo nano /etc/apt/sources.list
```
1.3. Paste the following text
```
deb http://ports.ubuntu.com/ubuntu-ports/ jammy main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports/ jammy-updates main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports/ jammy-backports main restricted universe multiverse
deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted universe multiverse
```
1.4. Update the package
```
sudo apt update
```
1.5. Re install the Xorg
```
sudo apt install xserver-xorg-video-dummy
```
1.6. Remove the previous Xorg config file
```
sudo rm /etc/X11/xorg.conf
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
4. Reboot jetson (Important: If you set up xorg, you wont see the dislay using hdmi. make sure you can access it with ssh or other remote software!)
```
sudo reboot
```
### VNC Setup
5. Setup x11vnc
```
sudo apt install x11vnc
```
6. Setup avahi
```
sudo apt install avahi-daemon avahi-utils
```
7. Create system service for x11vnc
```
sudo nano /etc/systemd/system/x11vnc.service
```
8. Paste
```
[Unit]
Description=x11vnc VNC Server
After=network.target multi-user.target
Wants=multi-user.target

[Service]
Type=simple  # Changed from forking to simple
User=jetsonN
Environment="DISPLAY=:0"
Environment="XAUTHORITY=/home/jetsonN/.Xauthority"
ExecStart=/usr/bin/x11vnc -display :0 -forever -shared -rfbauth /etc/x11vnc.pass
# Removed -bg flag for Type=simple
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```
9. Create password
```
sudo x11vnc -storepasswd /etc/x11vnc.pass
```
```
# Check ownership
sudo ls -la /etc/x11vnc.pass

# Should be owned by jetson2
sudo chown jetson2:jetsonN /etc/x11vnc.pass
sudo chmod 600 /etc/x11vnc.pass
```
10. Enable and start
```
sudo systemctl daemon-reload
sudo systemctl enable x11vnc
sudo systemctl start x11vnc
sudo systemctl status x11vnc
```
11. Reboot
```
sudo reboot
```
12. Check status
```
sudo systemctl status x11vnc
ps aux | grep x11vnc
```

## Setup RTC Clock
1. Make sure battery are installed at RTC and RTC are ready
```
ls -l /dev/rtc*
```
2. Usually RTC battery is set to ```rtc0```
3. Remove possible fake clock counter
```
sudo apt-get remove fake-hwclock
sudo dpkg --purge fake-hwclock
```
4. Set date and time accordingly
```
sudo date -s "YYYY-MM-DD HH:MM:SS"
```
5. Force write battery RTC
```
sudo hwclock -w -f /dev/rtc0
```
6. Read bettery RTC
```
sudo hwclock -r -f /dev/rtc0
```
7. Shutdown it
```
sudo shutdown now
```
8. Wait more than 1 minutes to ensure the RTC are counting.
9. Turn it on
10. Check the clock
```
sudo hwclock -r -f /dev/rtc0
```
