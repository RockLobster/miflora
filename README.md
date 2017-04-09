# Xiaomi mi flora

Script to read Xiaomi Flora values and publish it via MQTT to be used in Openhab or other home automation

It will scan for all Xiaomi miflora devices and publish the values to the MQTT server.

Paho (mqtt) and gatt libraries are required.

to install :
```
sudo apt-get install python-pip
pip install paho-mqtt
pip install gattlib
```

this requires Bluez 5.37 or higher to be installed as well

To run this as non-root run the following
```
sudo setcap cap_net_raw+eip $(eval readlink -f `which python`)
```

to run this in an automated way every 30 min add to the crontab the script:
```
$ crontab -e
```
add the line:
```
*/30 * * * * /usr/bin/python /home/ubuntu/miflower.py >/dev/null 2>&1
```
(replace the last part with your file name & path)

Based on the script created by emeryth https://wiki.hackerspace.pl/projects:xiaomi-flora

OpenHAB config
--------------
Sample *miflora.items* config: 

```
String MiFlora_One_Firmware "Firmware: [%s]" <firmware> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/firmware:state:default]"}
Number MiFlora_One_Battery "Battery [%d %%]" <battery> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/battery:state:default]"}
Number MiFlora_One_Temperature "Temperature [%.1f C]" <temperature> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/temperature:state:default]"}
Number MiFlora_One_Sunlight "Sunlight [%d lux]" <sunlight> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/sunlight:state:default]"}
Number MiFlora_One_Moisture "Moisture [%.2f %%]" <moisture> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/moisture:state:default]"}
Number MiFlora_One_Fertility "Fertility [%d uS/cm]" <fertility> (miflora_one) {mqtt="<[broker:/miflower/C47C8D61B4B9/fertility:state:default]"}

String MiFlora_Two_Firmware "Firmware: [%s]" <firmware> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/firmware:state:default]"}
Number MiFlora_Two_Battery "Battery [%d %%]" <battery> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/battery:state:default]"}
Number MiFlora_Two_Temperature "Temperature [%.1f C]" <temperature> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/temperature:state:default]"}
Number MiFlora_Two_Sunlight "Sunlight [%d lux]" <sunlight> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/sunlight:state:default]"}
Number MiFlora_Two_Moisture "Moisture [%.2f %%]" <moisture> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/moisture:state:default]"}
Number MiFlora_Two_Fertility "Fertility [%d uS/cm]" <fertility> (miflora_two) {mqtt="<[broker:/miflower/C47C8D624034/fertility:state:default]"}
```
1. replace `broker` with whatever it is called in your **mqtt.cfg** and 
2. replace `C47C8D61B4B9` and `C47C8D624034` with the mac addresses that you are interested in.

Content for sitemap:

```
    Frame label="Mi Flora 1" {
        Text item=MiFlora_One_Firmware
        Text item=MiFlora_One_Battery
        Text item=MiFlora_One_Temperature
        Text item=MiFlora_One_Sunlight
        Text item=MiFlora_One_Moisture
        Text item=MiFlora_One_Fertility
    }

    Frame label="Mi Flora 2" {
        Text item=MiFlora_Two_Firmware
        Text item=MiFlora_Two_Battery
        Text item=MiFlora_Two_Temperature
        Text item=MiFlora_Two_Sunlight
        Text item=MiFlora_Two_Moisture
        Text item=MiFlora_Two_Fertility
    }
```