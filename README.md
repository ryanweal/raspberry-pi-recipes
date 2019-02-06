# raspberry-pi-recipes
My raspberry pi recipes


## Pi-Hole

An ads-free and malware-free wifi network for every device to enjoy!

1. Download "lite": https://www.raspberrypi.org/downloads/raspbian/
2. Flash the SD card: https://www.raspberrypi.org/documentation/installation/installing-images/linux.md

    ```dd bs=4M if=2018-11-13-raspbian-stretch.img of=/dev/sdX conv=fsync```
    
    ```# note: may need to fully unmount first time, do not do /dev/sdb1 here (must be root of device)```
    
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



## E-ink PaperTTY pi

1. Lite
2. Flash
3. Boot with keyboard, hdmi, waveshare 2.13" connected and flash card loaded.
4. Login
5. Change pass
6. Enable SSH, SPI, I2C
7. Run updates
8. Install git: apt-get install git
9. mkdir -p /home/pi/code && cd /home/pi/code
10. git clone https://github.com/joukos/PaperTTY.git
11. sudo apt install virtualenvwrapper python3-virtualenv libopenjp2-7
12. source /usr/share/virtualenvwrapper/virtualenvwrapper.sh
13. add above line to .bashrc
14. still in PaperTTY directory: mkvirtualenv -p /usr/bin/python3 -r requirements.txt papertty
15. apt-get install python-libtiff?
16. screw it, use it without virtualenv: sudo apt install python3-rpi.gpio python3-spidev python3-pil python3-click
17. sudo ./papertty.py list
18. sudo ./papertty.py --driver epd2in13 scrub
19. sudo ./papertty.py --driver epd2in13 terminal # works with physical keyboard
20. apt-get install tmux
21. in another ssh window after terminal is running: sudo openvt -fc 1 -- sudo -u pi tmux
22. tmux attach
...
23. (on different machine) sudo apt install xfonts-terminus
24. (on different machine, with PI's IP)  scp ter-u12b_unicode.pcf.gz pi@192.168.1.8:/home/pi/code/PaperTTY
25. gunzip -c /usr/share/fonts/X11/misc/ter-u12b_unicode.pcf.gz > terminus-12.pcf # note the -c
26. pilfont.py terminus-12.pcf
27. sudo ./papertty.py --driver epd2in13 terminal --autofit --font terminus-12.pil --noclear --nocursor --scrub
