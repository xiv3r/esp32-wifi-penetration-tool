# <h1 align="center"> ESP32 Wi-Fi Penetration Tool</h1>

<p align="center">This project introduces an universal tool for ESP32 platform for implementing various Wi-Fi attacks. It provides some common functionality that is commonly used in Wi-Fi attacks and makes implementing new attacks a bit simpler. It also includes Wi-Fi attacks itself like capturing PMKIDs from handshakes, or handshakes themselves by different methods like starting rogue duplicated AP or sending deauthentication frames directly, etc...

Obviously cracking is not part of this project, as ESP32 is not sufficient to crack hashes in effective way. The rest can be done on this small, cheap, low-power SoC.</p>

<p align="center">
    <img src="doc/images/logo.png" alt="Logo">
</p>

## Features
- **PMKID capture**
- **WPA/WPA2 handshake capture** and parsing
- **Deauthentication attacks** using various methods
- **Denial of Service attacks**
- Formatting captured traffic into **PCAP format**
- Parsing captured handshakes into **HCCAPX file** ready to be cracked by Hashcat
- Passive handshake sniffing
- Easily extensible framework for new attacks implementations
- Management AP for easy configuration on the go using smartphone for example
- And more...

### Demo video

[![Demonstration Youtube video](https://img.youtube.com/vi/9I3BxRu86GE/0.jpg)](https://www.youtube.com/watch?v=9I3BxRu86GE)


* use this settings to force the clients reconnect to wifi
  
<p>
  <img align="center" width="480" height="700" src="https://github.com/xiv3r/esp32-wifi-penetration-tool/assets/117867334/b610d659-b778-45fb-ba4f-5295266b3f7a"
  </p>
    

* reconnect to the ManagementAP and browse 192.168.4.1
* the client will reconnect from his wifi
* .HCCAPX data will display download it and proceed.


<p>
  <img align="center" width="480" height="700" src="https://github.com/xiv3r/esp32-wifi-penetration-tool/assets/117867334/16fee414-082c-4e91-9e84-5de315240fb9)"
  </p>


#### Bruteforce Using Android: [HCCAPX Bruteforce](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/HCCAPX%20Bruteforce.apk) & [Wordlist](https://github.com/xiv3r/esp32-wifi-penetration-tool/releases/download/Wordlists/rockyou.txt)

https://github.com/xiv3r/esp32-wifi-penetration-tool/assets/117867334/717b7602-d552-4e01-9f23-acff19464ffd


BruteForce .hccapx using Windows [Hash Suite Droid](https://hashsuite.openwall.net/downloads/Hash_Suite_Free_3_7.zip)

* Linux:

###### Aircrack-ng
    aircrack-ng (directory)/handshake.hccapx -w (director)/Downloads/rockyou.txt



## Usage

1. [Build](#Build) and [flash](#Flash) project onto ESP32 (DevKit or module)
2. Power ESP32
3. Management AP is started automatically after boot
4. Connect to the Wi-Fi
   
*Wi-Fi SSID:* `ManagementAP` and *Password:* `mgmtadmin`

In browser open `192.168.4.1` and you should see a web client to configure and control tool like this:

![Web client UI](doc/images/ui-config.png)

## Build
This project is currently developed using ESP-IDF 4.1 (commit `5ef1b390026270503634ac3ec9f1ec2e364e23b2`). It may be broken on newer version.

Project can be built in the usual ESP-IDF way:

```shell
idf.py build
```

Legacy method using `make` is not supported by this project.

## Flash

### Download:

1. [`0x1000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/bootloader/bootloader.bin)
2. [`0x8000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/partition_table/partition-table.bin)
3. [`0x10000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/esp32-wifi-penetration-tool.bin)

If you have setup ESP-IDF, the easiest way is to use `idf.py flash`.

In case you don't want to setup whole ESP-IDF, you can use pre-build binaries included in [`build/`](build/) and flash them using [`esptool.py`](https://github.com/espressif/esptool) (requires Python).

* Example command (follow instructions in [esptool repo](https://github.com/espressif/esptool)):

      sudo apt update
      sudo apt install esptool
 
* Install from Repo:
  ```
  wget http://ftp.de.debian.org/debian/pool/main/e/esptool/esptool_2.8+dfsg-1_all.deb
  
  cd Downloads

  sudo dpkg -i esptool_2.8+dfsg-1_all.deb
  
* Fix Missing Dependencies:
  ```
  sudo apt --fix-broken install -f -y
  ```
  ```
  esptool -b 115200 --after hard_reset write_flash --flash_mode dio --flash_freq 40m --flash_size detect 0x8000 build/partition_table/partition-table.bin 
  0x1000 build/bootloader/bootloader.bin 0x10000 build/esp32-wifi-penetration-tool.bin
  ```
![Screenshot_20231020_104352](https://github.com/xiv3r/esp32-wifi-penetration-tool/assets/117867334/4def4b5e-7c76-4544-b8f2-d757b14b2f96)


On Windows you can Download [Flash Download Tool](https://www.espressif.com/sites/default/files/tools/flash_download_tool_3.9.5.zip)

* ###### Choose `ESP32` only
* ###### Boot Partiton [`0x1000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/bootloader.bin)
* ###### Partition Table [`0x8000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/partition_table/partition-table.bin)
* ###### ESP32 Partition [`0x10000`](https://github.com/xiv3r/esp32-wifi-penetration-tool/raw/master/build/esp32-wifi-penetration-tool.bin)
* Drivers:
##### [CP2102](https://github.com/xiv3r/esp32-wifi-penetration-tool/files/13049286/CP210x_Windows_Drivers.zip)
##### [CH340G](https://github.com/xiv3r/esp32-wifi-penetration-tool/files/13049308/CH340G.zip)


<p>
  <img align="center" height="700" src="https://github.com/xiv3r/esp32-wifi-penetration-tool/assets/117867334/d1a15e2c-b50f-4473-8069-2bdf5bd43776"
 </p>


## Documentation

* Wi-Fi attacks
Attacks implementations in this project are described in [main component README](main/). Theory behind these attacks is located in [doc/ATTACKS_THEORY.md](doc/ATTACKS_THEORY.md)
### API reference
This project uses Doxygen notation for documenting components API and implementation. Doxyfile is included so if you want to generate API reference, just run `doxygen` from root directory. It will generate HTML API reference into `doc/api/html`.

### Components
This project consists of multiple components, that can be reused in other projects. Each component has it's own README with detailed description. Here comes brief description of components:

- [**Main**](main) component is entry point for this project. All neccessary initialisation steps are done here. Management AP is started and the control is handed to webserver.
- [**Wifi Controller**](components/wifi_controller) component wraps all Wi-Fi related operations. It's used to start AP, connect as STA, scan nearby APs etc. 
- [**Webserver**](components/webserver) component provides web UI to configure attacks. It expects that AP is started and no additional security features like SSL encryption are enabled.
- [**Wi-Fi Stack Libraries Bypasser**](components/wsl_bypasser) component bypasses Wi-Fi Stack Libraries restriction to send some types of arbitrary 802.11 frames.
- [**Frame Analyzer**](components/frame_analyzer) component processes captured frames and provides parsing functionality to other components.
- [**PCAP Serializer**](components/pcap_serializer) component serializes captured frames into PCAP binary format and provides it to other components (mostly for webserver/UI)
- [**HCCAPX Serializer**](components/hccapx_serializer) component serializes captured frames into HCCAPX binary format and provides it to other components (mostly for webserver/UI)

### Further reading
* [Academic paper about this project (PDF)](https://excel.fit.vutbr.cz/submissions/2021/048/48.pdf)

## Hardware 
This project was mostly build and tested on **ESP32-DEVKITC-32E**
but there should not be any differences for any **ESP32-WROOM-32** modules.

<p align="center">
    <img src="doc/images/soucastky_8b.png" alt="Hw components" width="400">
</p>

On the following pictures you can see a battery (Li-Pol accumulator) powered ESP32 DevKitC using following hardware:
- **ESP32-DEVKITC-32E** (cost 213 CZK/8.2 EUR/9.6 USD)
- 220mAh Li-Pol 3.7V accumulator (weights ±5g, cost 77 CZK/3 EUR/3.5 USD)
- MCP1702-3302ET step-down 3.3V voltage regulator (cost 11 CZK/0.42 EUR/0.50 USD)
- Czech 5-koruna coin for scale (weights 4.8g, diameter 23 mm, cost 0.19 EUR/0.23 USD)
<p align="center">
    <img src="doc/images/mini.jpg" alt="Hw components" width="300">
    <img src="doc/images/mini2.jpg" alt="Hw components" width="300">
</p>

Altogether (without coin) this setup weights around 17g. This can be further downsized by using smaller Li-Pol accumulator and using ESP32-WROOM-32 modul directly instead of whole dev board.

This setup cost me around 300 CZK (± 11.50 EUR/13.50 USD). Using the modul directly that costs around 80 CZK (± 3 EUR/3.5 USD) we can get to price of 160 CZK (± 6.5 EUR/7.5 USD) which makes this tool really cheap and available to almost everybody.

### Power consumption
Based on experimental measurements, ESP32 consumes around 100mA during attack executions. 

## Similar projects
* [GANESH-ICMC/esp32-deauther](https://github.com/GANESH-ICMC/esp32-deauther)
* [SpacehuhnTech/esp8266_deauther](https://github.com/SpacehuhnTech/esp8266_deauther)
* [justcallmekoko/ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder)
* [EParisot/esp32-network-toolbox](https://www.tindie.com/products/klhnikov/esp32-network-toolbox/)
* [Jeija/esp32free80211](https://github.com/Jeija/esp32free80211)

## Contributing
Feel free to contribute. Don't hestitate to refactor current code base. Please stick to Doxygen notation when commenting new functions and files. This project is mainly build for educational and demonstration purposes, so verbose documentation is welcome.

## Disclaimer
This project demonstrates vulnerabilities of Wi-Fi networks and its underlaying 802.11 standard and how ESP32 platform can be utilised to attack on those vulnerable spots. Use responsibly against networks you have permission to attack on.

## License
Even though this project is licensed under MIT license (see [LICENSE](LICENSE) file for details), don't be shy or greedy and share your work.
