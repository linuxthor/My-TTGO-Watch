# Hackers-TTGO-Watch

![time](images/main.png?raw=true "time")
![apps](images/watch.png?raw=true "apps")
![iplookup](images/iplookup.png?raw=true "iplookup")
![bluebox](images/bluebox.png?raw=true "bluebox")
![ping](images/ping.png?raw=true "ping")
![portscan](images/portscan.png?raw=true "portscan")

An alternative software distribution for the TTGO smartwatch based on ESP32 from LilyGo. Currently under active development (check back often for new functionality) 

Forked from the excellent software by sharandac https://github.com/sharandac/My-TTGO-Watch with some small fixes and different apps. 

* Changed main font (e.g for the timeface) to FreeMono (TODO: Fix naming. Still appears as 'ubuntu')
* Removed crypto ticker
* Removed weather app
* Removed FTP server (TODO: will try to add SSH server for SCP) 
* Removed osmand mapping app
* Removed some of the haptic motor stuff
* Removed update feature for now (TODO: probably to update over SSH) 
* Fixed a sound issue with wav/mp3 playback

# Install

Clone this repository and open it with platformIO. Build and upload. On a terminal in vscode you can do it with

```bash
pio run -t upload
```

or simple press "build and upload" in platformIO.

The first time you may wish to add bluebox mp3 files with:

```    
pio run -t uploadfs     
```    

# known issues

* the battery indicator is not accurate, rather a problem with the power management unit ( axp202 )    
~SSH client sometimes (after a while) produces false 'connection error' and watch needs to be rebooted to restore app to working~ (hopefully resolved)          
* after using the wifi monitor may need to turn wifi back on afterwards to use normal functions    
* from time to time the esp32 crashes accidentally    
* and some other small things    

# Improving battery life / stability

* I have found it best for battery and stability to use just the wifi and keep the bluetooth disabled for now     
* I set a plain black background via the settings which seems to make the screenshot process more stable (still some reboots)    
* Seems best to disable webserver when not in use    

# Apps    

## New:-    
*Bluebox*     - bluebox for the phreaks (add the included mp3 files to SPIFFS using `pio run -t uploadfs` the first time)     
*Netscan*     - Simple connect() based port scanner (slow with unreachable/blocked hosts - is there a better way working for ESP32?)    
*Subnet*      - Simple subnet calculator to see how many hosts / valid hosts    
*Ping*        - Ping some IP address     
*IP Lookup*   - Lookup who owns an IP via the ip-info API    
*SSH client*  - Password only for now (no keys - TODO) / (known hosts handling via SPIFFS - TODO)      
*SSID Spam*   - Advertise some wireless networks named like Rick Astley lyrics     
*WiFi mon*    - See how busy a wireless channel is      

## Existing:-
*IR Remote*   - IR remote     
*Stopwatch*   - Stopwatch     
*Alarm*       - Alarm clock     

# Call for ideas    

If you have something you would like to see built for the watch (especially network/security tools) then please let me know using github issues. ~I will also make a tutorial about how to program your own apps shortly.~ (Now started here: https://github.com/linuxthor/My-TTGO-Watch-Dev)           
 
# For the programmers

Internal RAM is very limited, use PSRAM as much as possible. When you work with ArduinoJson, include this

```#include "hardware/json_psram_allocator.h"```

and create your json with

```SpiRamJsonDocument doc( 1000 );```

to move your json into PSRAM, here is enough RAM for all the crazy stuff you will do. And use

```ps_malloc(), ps_calloc() and ps_realloc()```

as often as possible.
And one very important thing: Do not talk directly to the hardware!

## Sound
To play sounds from the inbuild speakers use `hardware/sound.h`:

```
#include "hardware/sound.h"
[...]
// MP3 from SPIFFS:
// void sound_play_spiffs_mp3( const char *filename );
// example:
sound_play_spiffs_mp3( "/sound.mp3" )

// or WAV from PROGMEM via
//void sound_play_progmem_wav( const void *data, uint32_t len );

```

There is a configuration tile to enable/disable all sound output and set the global volume.

# how to make a screenshot
The firmware has an integrated webserver. Over this a screenshot can be triggered. The image has the format RGB565 and can be read with gimp. From bash it looks like this
```
wget x.x.x.x/shot && wget x.x.x.x/screen.data
```
I use the following import settings for GIMP:   
* rgb565
* offset 1
* width 240
* height 240    

# Contributors 

Massive thanks to [sharadac](https://github.com/sharandac/) for the TTGO Watch project.<br> 

Where they give special thanks to the following people for their help:

[5tormChild](https://github.com/5tormChild)<br>
[bwagstaff](https://github.com/bwagstaff)<br>
[chrismcna](https://github.com/chrismcna)<br>
[datacute](https://github.com/datacute)<br>
[jakub-vesely](https://github.com/jakub-vesely)<br>
[joshvito](https://github.com/joshvito)<br>
[JoanMCD](https://github.com/JoanMCD)<br>
[NorthernDIY](https://github.com/NorthernDIY)<br>
[rnisthal](https://github.com/rnisthal)<br>
[paulstueber](https://github.com/paulstueber)<br>
[ssspeq](https://github.com/ssspeq)<br>

Thanks also to the following projects:

[ArduinoJson](https://github.com/bblanchon/ArduinoJson)<br>
[AsyncTCP](https://github.com/me-no-dev/AsyncTCP)<br>
[ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer)<br>
[LVGL](https://github.com/lvgl)<br>
[TFT_eSPI](https://github.com/Bodmer/TFT_eSPI)<br>
[TTGO_TWatch_Library](https://github.com/Xinyuan-LilyGO/TTGO_TWatch_Library)<br>
[ESP8266Audio](https://github.com/earlephilhower/ESP8266Audio)<br>
[pubsubclient](https://github.com/knolleary/pubsubclient)<br>
[ESP32Ping](https://github.com/marian-craciunescu/ESP32Ping)<br>
[LibSSH-ESP32](https://github.com/ewpa/LibSSH-ESP32)<br>

Every Contribution to this repository is highly welcome! Don't fear to create pull requests which enhance or fix the project, you are going to help everybody.
