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

## E-ink bitmap

- do not install bcm driver or wiredpi

https://github.com/soonuse/epd-library-python
python-spidev
python-pil
ttf-freefont

- Create a loader using the example:

```
import epd2in13
import time
from PIL import Image
from PIL import ImageDraw
from PIL import ImageFont

def main():
    epd = epd2in13.EPD()
    epd.init(epd.lut_full_update)

    image = Image.open('weather.bmp')
    draw = ImageDraw.Draw(image)
    font = ImageFont.truetype('/usr/share/fonts/truetype/freefont/FreeMonoBold.ttf', 12)
    draw.rectangle((0, 240, 130, 250), fill = 0)
    draw.text((20, 239), 'Hello world!', font = font, fill = 255)
   
    epd.clear_frame_memory(0xFF)
    epd.set_frame_memory(image, 0, 0)
    epd.display_frame()

if __name__ == '__main__':
    main()
```

## E-ink bitmap scrape and display bitmap generator

- install nvm
- nvm install node --lts
- install puppeteer script + cards nuxt folder
- configure template in nuxt (cards/pages/weather.vue as example)...
- then `npm run generate` to create cards/dist folder
- install the webserver: `npm i -g http-server`
- make sure port numbers match index.js and the port in do-it.sh
- npm i puppeteer # it is a different processor type so don't rsync your node_modules
- install debian dependencies necessary for headless chrome:
  - ```sudo apt install gconf-service libasound2 libatk1.0-0 libatk-bridge2.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils wget```
  - that doesn't work.
  - sudo apt install chromium-browser
  - chromium-codecs-ffmpeg
  - add to headless config
  - sudo apt install imagemagick
 - puppeteer is not my friend right now, different models of pi have different ARM architecture... hmm.
 - puppeteer-core@v1.11.0 "stable" is the one that works with chromium-browser version 65, which is the default in Stretch
 - const puppeteer = require('puppeteer-core');        
 - const browser = await puppeteer.launch({executablePath: '/usr/bin/chromium-browser'});
 - ... but the site I was scraping has changed selector names today. So it isn't perfect yet.
 - cleaned up my custom code... "it works now" https://github.com/ryanweal/papercards (sorta, mostly).
 - debugging cron
   - `sudo apt install sendmail mutt`
