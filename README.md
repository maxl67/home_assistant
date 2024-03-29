# Home-Assistant Configuration
## Environment/Infrastructure
I run Home-Assistant core in a docker container (homeassistant/home-assistant from dockerhub) on a Synology NAS. 
A Zigbee coordinator (USB stick NORTEK HUSBZB-1) is plugged into NAS directly.
I generally try to avoid using any vendor specific gateways and keep all of my data in my network at home. The only exception is a virtual interface to the HomeMatic environment which I run on a separate Raspberry Pi.

## Structure of HA Config
I try to combine all code for a specific topic into a package. Integrations and hardware specific stuff gets its own package (like homematic, synology).

The main topics that I am handling with Home-Assistant are:
### Presence Detection
I mainly use bluetooth for device tracking. I run [Andrew Freyers super cool script](https://github.com/andrewjfreyer/monitor) on 2 Rasperry Pi's to cover the whole appartment. Some automations will reduce the amount of arrival and departure scans to a minimum. Arrival is usually detected within 2-5 seconds, departure in less that a minute. I got the idea to have different occupancy status from [Phil Hawthorne](https://philhawthorne.com/making-home-assistants-presence-detection-not-so-binary).
- Packages:   [presence_detection](packages/presence_detection.yaml), 
- Views: [home_view](lovelace/views/home_view.yaml)

### Open Doors/Windows
I will get a notification on my device as soon as I leave the house and a window or backdoor is open. In addition a lightstrip is signaling the status whenever I open my entrance door (red: backdoor is open, yellow: window is open, green: doors and windows are closed). I use XIAOMI mi Aqara door sensors (ZigBee protocol).
- Packages:   [doors_windows](packages/doors_windows.yaml), [notification](packages/notification.yaml)
- Views: [home_view](lovelace/views/home_view.yaml)

### Vacuum Robot
My XIAOMI Roborock S6 will automatically clean the appartment when the last person leaves. Cleaning is done on specific days and for different areas. First person returning home will get a notification to clean the dustbin. Robot can be started via HA for room or complete cleanup.
- Packages:   [vacuum](packages/vacuum.yaml)
- Views: [vacuum_view](lovelace/views/vacuum_view.yaml)

### Automation of heating
Radiators will be turned off as soon as the last person leaves the appartment. Heating will also be turned off in a room, whenever a window/door is opened in that room. Different heating shedules (profiles) will be set automatically for regular work days, home office days or when guests are present. I use HomeMatic IP thermostats. The virtual HomeMatic CCU (piVCCU) runs on one of my rasperry pi's.
- Packages:   [climate](packages/climate.yaml), [HomeMatic](packages/homematic.yaml)
- Views: [climate_view](lovelace/views/climate_view.yaml)

### Automation of blinds and awnings
Blinds and awnings are wired to HomaticIP Wired components. Blinds are automatically lowered on a side (west/south) whenever the sun is shining (on that side). They are retracted when the sun direction leavees this side. Blinds and awnings are also retracted when wind exceeds a threshold. 
Sunshine is detected from the power output of a plug-in solar panel.
- Packages:   [covers], [weather](packages/weather.yaml)

### Weather Forecast and Warnings
Warnings from the german weather service (DWD integration) are analysed and in case of storm and rain e-mail messages and notifications are send. In the furture I will use this information to automate outside blinds and awnings (as soon as I get them connetced to HA).
- Packages:   [weather](packages/weather.yaml)
- Views: [weather_view](lovelace/views/weather_view.yaml)

### Automation of Power Consumption and Lights
A welcome light is automatically turned on upon arriving at home or at sunset and turned off when leaving the appartment. Some power plugs are also turned on and off depending on presence. Devices use Zigbee or DECT protocoll.
- Packages:   [power](packages/power.yaml)
- Views: [home_view](lovelace/views/home_view.yaml)

### Monitoring Devices
Information about internet connection and the status of some network devices is monitored. 
- Packages:   [synology](packages/synology.yaml), [network](packages/network.yaml), [fritzbox](packages/fritzbox.yaml)
- Views: [monitoring_view](lovelace/views/monitoring_view.yaml)
