# ESP8266-Captive-Portal

ESP8266 Captive Portal with Fake Google login page.

![loginpage](https://github.com/beigeworm/ESP8266-Captive-Portal/assets/93350544/7b6f2366-97d1-4668-aea4-a1b2ed8024cb)


# INSTALL INSTRUCTIONS

**ESPBug**
ESPBug is a rogue captive portal program which runs on the ESP8266 dev board, such as the NodeMCU (clones included). It is a social engennering tool which generates a WiFi network of a given name alluring people to connect to it and enter some credential.

The compiled binary is in the source files as ESP8266 Captive Portal.BIN 

and just updload the binary through the arduino IDE. Look below to see how to setup the board.
From source

**Install from source**

You need to install the Arduino IDE 

From there you need a couple installations clicking the upload button. First got File -> Preferences. At the bottom of the window, you should see Additional Boards Manager URLS. Click the little icon at the end at the end of that line. A new window should show up asking you to Enter additional URLS, one for each row. Add:

> https://arduino.esp8266.com/stable/package_esp8266com_index.json

And click OK.

Next, in library manager (Sketch -> Include Library -> Manage Libraries) search for

`ArduinoJson`

From Benoit Blanchon. YOU MUST INSTALL VERSION 5.13.5 not version 6.

Now you only need to install the board. Tools -> Board: "<SOME BOARD NAME>" -> Boards Manager. In the search bar, type

`esp8266`

by ESP8266 Community. I installed version 2.5.5 2.6.0.

And YOU ARE DONE! Now, you need to open the espbug.ino and setup the correct parameters for the board.

Make sure to change the default credentials in "config.h"

**DEFAULT CREDENTIALS** (for the precompiled .bin file)
> SSID - monbug
> 
> PASS - pwntoown


**Convert webpages** (Not A Required Step)

The web pages are saved in a compress form and in bytes. A small script is available to convert you website into the correct format. this runs in Python3. In the web_converter you must add your web pages in web_pages:

\web_converter>tree /F
Folder PATH listing for volume HDD
Volume serial number is 5E0C-E860
D:.
│   requirements.txt
│   webConverter.py
│
├───css_html_js_minify
│   │   [...]
│
└───web_pages
        example.html

Then you just need to run the python script knowing that anglerfish must be installed (python -m pip install anglerfish).

\web_converter>PYTHON webConverter.py

Then you'll need to recompile the project and upload it to the board. I know, it's not elegante, but it works.

**Credits**

CREDITS where credit is due

This Project is based on ESP-Bug by : https://github.com/willmendil

Most of the code is strongly inspired by different repo from github, I just wanted to mention them here.

    ESPortalV2 - https://github.com/exploitagency/ESPortalV2
    github-ESPortal - https://github.com/exploitagency/github-ESPortal
    esp8266_deauther - https://github.com/spacehuhn/esp8266_deauther
    and maybe more which I'll add if I remember
