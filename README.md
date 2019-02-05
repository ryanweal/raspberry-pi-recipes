# raspberry-pi-recipes
My raspberry pi recipes


## Pi-Hole

An ads-free and malware-free wifi network for every device to enjoy!

1. Download "lite": https://www.raspberrypi.org/downloads/raspbian/
2. Flash the SD card: https://www.raspberrypi.org/documentation/installation/installing-images/linux.md

    ```dd bs=4M if=2018-11-13-raspbian-stretch.img of=/dev/sdX conv=fsync
    
    # note: may need to fully unmount first time, do not do /dev/sdb1 here (must be root of device)```
    
3. Boot with HDMI and keyboard and ethernet connected, sd card loaded before power.
4. User pi pass raspberry
5. Change password
6. Enable SSH
7. Run updates
8. Run pi-hole curl command: https://github.com/pi-hole/pi-hole/#one-step-automated-install

    ```curl -sSL https://install.pi-hole.net | bash```

9. Edit router settings, set current as fixed IP, accept pi-hole network defaults after confirming
10. Set router to use pi-hole IP as the DNS

It took me about an hour to do this recipe. It could probably be done in much less time.

I copied down the wrong password for the web UI but when I did "forgot password" it gave me a command to run to reset it. That was helpful.
