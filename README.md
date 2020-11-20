# Home-Assistant Configuration
## Environment/Infrastructure
I run Home-Assistant core in a docker container (homeassistant/home-assistant from dockerhub) on a Synology NAS. 
Zigbee Coordinator (USB stick NORTEK HUSBZB-1) is plugged into NAS directly.
I generally try to avoid using any vendor specific gateways and keep all of my data in my network at home.

## Structure
I try to combine all code for a specific topic into a package. Integrations and hardware specific stuff gets its own package (like homematic, synology).

The main topics that I am handling with Home-Assistant are:
### Presence Detection
I mainly use bluetooth for device tracking. I run [Andrew Freyers super cool script](https://github.com/andrewjfreyer/monitor) on 2 Rasperry Pi's to cover the whole appartment. Some automations will reduce the amount of arrival and departure scans to a minimum. Arrival is usually detected within 2-5 seconds, departure in less that a minute.
- Packages:   [presence_detection](packages/presence_detection.yaml), 
- Views: [home_view](lovelace/views/home_view.yaml)

### Open Doors/Windows
I will get a notification on my device as soon as I leave the house and a window or door is open. In addition a lightstrip is signaling the status whenever I open my entrance door (red: door is open, yellow: window is open, green: doors and windows are closed)
- Packages:   [doors_windows](packages/doors_windows.yaml), [notification](packages/notification.yaml)
- Views: [home_view](lovelace/views/home_view.yaml)

### Vacuum Robot
My XIAOMI Roborock S6 will automatically clean the appartment when the last person leaves. Cleaning is done on specific days and for different areas. First person returning home will get a notification to clean the dustbin. Robot can be started via HA for room or complete cleanup.
- Packages:   [vacuum](packages/vacuum.yaml)
- Views: [vacuum_view](lovelace/views/vacuum_view.yaml)

### Weather Forecast and Warnings
Warnings from the german weather service (DWD integration) are analysed and in case of storm and rain mail messages and notifications are send. In the furture I will use this information to automate outside blinds and awnings (as soon as I get them connetced to HA).
- Packages:   [weather](packages/weather.yaml)
- Views: [weather_view](lovelace/views/weather_view.yaml)

### Automation of Heating
Radiators will be turned off as soon as the last person leaves the appartment. Heating will also be stopped in a room, whenever a window/door is opened in that room. I use HomeMatic IP thermostats. The virtual HomeMatic CCU (piVCCU) runs on one of my raspis.
- Packages:   [climate](packages/climate.yaml), [HomeMatic](packages/homematic.yaml)
- Views: [climate_view](lovelace/views/climate_view.yaml)

### Automation of Power Consumption and Lights
A welcome light is automatically turned on upon arriving at home or at sunset and turned off when leaving the appartment. Some power plugs are also turned on and off depending on presence. All devices use zigbee protocoll.
- Packages:   [power](packages/power.yaml)
- Views: [home_view](lovelace/views/home_view.yaml)

### Monitoring Devices
Information about internet connection and the status of some network devices is monitored. I also use [NotoriousBDG's Battery Alert Package](https://raw.githubusercontent.com/notoriousbdg/Home-AssistantConfig/master/packages/battery_alert.yaml).

- Packages:   [synology](packages/synology.yaml), [network](packages/network.yaml), [fritzbox](packages/fritzbox.yaml)
- Views: [monitoring_view](lovelace/views/monitoring_view.yaml)
