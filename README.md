# Home-Assistant Configuration
##Environment/Infrastructure


##Environment/Infrastructure
I run Home-Assistant core in a docker container (homeassistant/home-assistant from dockerhub) on a Synology NAS. 
Zigbee Coordinator (USB stick NORTEK HUSBZB-1) is plugged into NAS directly.
I generally try to avoid using any vendor specific gateways and keep all of my data in my network at home.

##Structure
I try to combine all code for a specific topic into a package. Integrations and hardware specific stuff gets its own package (like homematic, synology).

The main topics that I am handling with Home-Assistant are:
###Presence Detection
I mainly use bluetooth for device tracking. I run Andrew Freyers script (https://github.com/andrewjfreyer/monitor) on 2 Rasperry Pi's to cover the whole appartment. Some automations will reduce the amount of arrival and departure scans to a minimum. Arrival is usually detected within 2-5 seconds, departure in less that a minute.
Packages:   presence_detection.yaml

###Open Doors/Windows
I will get a notification on my device as soon as I leave the house and a window or door is open. In addition a lightstrip is signaling the status whenever I open my entrance door (red: door is open, yellow: winow is open, green: doors and windows are closed)

###Vacuum Robot
My XIAOMI Roborock S6 will automaticly clean the appartment when the last person leaves the appartment. Cleaning is done on specific days and for different areas. First person returning home will get a notification to clean die dustbin. Robot can be started via HA for room or complete cleanup.

###Weather Forecast and Warnings
Warnings from the german weather service (DWD integration) are analysed and in case of storm and rain  mail messages and notifications are send. In the furture I will use this information to automate outside blinds and awnings (as soon as I get them connetced to HA).

###Automation of Heating
Radiators will be turned off as soon as the last person leaves the appartment. Heating will also be stopped in a room, whenever a window/door is opened in that room. I use HomeMatic IP thermostats. The virtual HomeMatic CCU (piVCCU) runs on one of my raspis.

###Automation of Power Consumtion and Lights
A welcome light is automatically turned on upon arriving at home or at sunset. It is turned off when leaving the appartment. Some power plugs are also turned on and off depending on presence. All devices use zigbee protocoll.

###Monitoring IT-Devices
Information about internet connection and the status of some network devices is monitored.
Packages:   synology.yaml, network.yaml, fritzboy.yaml
Views:  monitoring_view.yaml
