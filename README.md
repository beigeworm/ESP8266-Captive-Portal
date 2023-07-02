# ESP8266-Captive-Portal
ESP8266 Captive Portal with Google login page

This Project is based on ESP-Bug by : https://github.com/willmendil

**INSTALLATION INSTRUCTIONS**

**ESPBug**
ESPBug is a rogue captive portal program which runs on the ESP8266 dev board, such as the NodeMCU (clones included). It is a social engennering tool which generates a WiFi network of a given name alluring people to connect to it and enter some credential.

You should be able to download the binary from the release tab in github. https://github.com/willmendil/ESPBug/releases/tag/0.1 and just updload the binary through the arduino IDE. Look below to see how to setup the board.
From source

To get this code running on a nodeMCU like board - such as the one illustrated below - you need to install the Arduino IDE (! I have only tried on Linux, but this should work on any OS - virtual machine included).

esp8266 image

Download and install

From there you need a couple installations clicking the upload button. First got File -> Preferences. At the bottom of the window, you should see Additional Boards Manager URLS. Click the little icon at the end at the end of that line. A new window should show up asking you to Enter additional URLS, one for each row. Add:

https://arduino.esp8266.com/stable/package_esp8266com_index.json

and click OK.

Next, in library manager (Sketch -> Include Library -> Manage Libraries) search for

ArduinoJson

From Benoit Blanchon. YOU MUST INSTALL VERSION 5.13.5 not version 6.

nearly there

Now you only need to install the board. Tools -> Board: "<SOME BOARD NAME>" -> Boards Manager. In the search bar, type

esp8266

by ESP8266 Community. I installed version 2.5.5 2.6.0.

And YOU ARE DONE! Now, you need to open the espbug.ino and setup the correct parameters for the board.

Here are MY setup parameters

Board: "NodeMCU 1.0 (ESP-12E Module)"
Upload Speed: "115200"
CPU Frequency: "160 MHz"
Flash Size: "4M (FS:3MB OTA:~512KB)"
Debug port: "Disabled"
Debug Level: "None"
IwIP Variant: "v2 Lower Memory"
VTables: "Flash"
Exceptions: "Disabled (new can abort)"
Erase Flash: "All Flash Contents"
SSL Support: "All SSL ciphers (most compatible)"
Port : "<USB PORT>"

Convert webpages

you can look at this video for quick demo https://youtu.be/1cIXfD_Jz5s

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

webConverter for master

p <PATH TO>\ESPBug\web_converter
parent <PATH TO>
q compressed
arduino_file_path <PATH TO>\ESPBug\web_converter\webfiles.h
datadir <PATH TO>\ESPBug\web_converter\web_pages
dir <PATH TO>\web_interface
datadir <PATH TO>\ESPBug\web_converter\web_pages
compressed <PATH TO>\ESPBug\web_converter\web_pages\compressed
filelist []
html_files [WindowsPath('<PATH TO>ESPBug/web_converter/web_pages/example.html')]
css_files []
js_files []
[+] Minifying example.html...
[+] Compressing example.html...
[+] Saving everything into webfiles.h...

[+] Done, happy uploading :)
Here are the updated functions

server.on(String(F("/example.html")).c_str(), HTTP_GET, [](){
  sendProgmem(examplehtml, sizeof(examplehtml), W_HTML);
});

This is the ouput, not very pretty, but functionnal. (I replaced the exact path to <PATH TO> for privacy). There is the code that must be added to the void startAP() function in servingWebPages.h. The string F("/example.html") is actually the url path that needs to be called to serve the page. You can therefore put anything you want here.

If we look at the tree we see new stuff now

\web_converter>tree /F
Folder PATH listing for volume HDD
Volume serial number is 5E0C-E860
D:.
│   requirements.txt
│   webConverter.py
│   webfiles.h
│
├───css_html_js_minify
│   │   [...]
│
└───web_pages
    │   example.html
    │
    └───compressed
            example.html.gz

Specifically a compressed folder was created in web_pages. You actually need to move everything in web_converter\web_pages inside the espbug folder were the espbug.ino resides.

\ESPBug>tree
Folder PATH listing for volume HDD
Volume serial number is 5E0C-E860
D:.
├───espbug
│   └───web_pages      <---- [In this folder]
│       ├───compressed              |
│       │   └───js                  |
│       ├───jade                    |
│       └───json                    |
└───web_converter                   |
    ├───css_html_js_minify          |
    └───web_pages       [MOVE THIS  |]
        └───compressed

Then you'll need to recompile the project and upload it to the board. I know, it's not elegante, but it works.
CREDITS where credit is due

Most of the code is strongly inspired by different repo from github, I just wanted to mention them here.

    ESPortalV2 - https://github.com/exploitagency/ESPortalV2
    github-ESPortal - https://github.com/exploitagency/github-ESPortal
    esp8266_deauther - https://github.com/spacehuhn/esp8266_deauther
    and maybe more which I'll add if I remember
