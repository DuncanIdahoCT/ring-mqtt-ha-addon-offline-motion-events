![ring-mqtt-logo](https://raw.githubusercontent.com/tsightler/ring-mqtt-ha-addon/master/logo.png)

![aarch64-shield](https://img.shields.io/badge/aarch64-yes-green.svg)
![amd64-shield](https://img.shields.io/badge/amd64-yes-green.svg)
![armhf-shield](https://img.shields.io/badge/armhf-yes-green.svg)
![armv7-shield](https://img.shields.io/badge/armv7-yes-green.svg)

# Fork About
This fork includes some custom code and dashboard template ideas that extend the functionality of this amazing add-on so you can have each motion event (both the snapshot image and video) downloaded from Ring.com and automatically refreshed onto your Home Assistant dashboards. The possibilities to extend this to include event selection is there but I only did the work to show the latest event.

![Dashboard Card](/offline-motion-events/Driveway.png)

Most of the credit goes to the contributors in this thread:
https://community.home-assistant.io/t/100-templatable-lovelace-configurations/105241/325
Both the creator of the config-template-card and the conversation where code is shown on how to use the state of an entity or input_text helper to automatically refresh HA dashboard cards when the event from Ring.com changes.

The unique features I've added is to pull down the video and snapshot image from the Ring.com cloud when the event ID changes, which means a new motion event has been recorded, and store them locally on the HA server, then refresh the dashboard with an input_text datetime stamp so that it's always as fast as possible and the offline video plays instantly instead of a long delay waiting for Ring.com API. The resulting automatically refreshing (it will change before your eyes when a motion event happens! :) ) works just as well locally as it does using nabucasa and in the Home Assistant mobile app. Oddly it looks nicer in the mobile app too... I'm sure if I know more about web css I could make it look as nice in desktop Chrome lovelace but I gave up trying to get rid of the white frame (iframe boarder) and it only now slightly bothers me lol.

# Custom code and how to use it:
* Note: my camera name in this example code is "driveway" so anyplace you see that reference you should change it to your Ring Camera Name as it appears in HA after you complete the initial(original) setup.
  
* After ensuring you have the (original) add-on working as per below, you'll need to setup a few things in HA and have access to the /config/www folder path on your HA install
1) In Home Assistant, navigate to HACS and install the frontend config-template-card
2) Copy the file: offline-motion-events/driveway_latest_event.html from this repo up to your HA config/www folder path
3) In your HA overview, click edit and then new card-scrolling to the very bottom and clicking manual
4) Paste in the contents from the offline-motion-events/grid-card-configuration.md file and save the card
5) In Home Assistant, create a helper of type "Text" and name it driveway_timestamp
6) In Home Assistant, create an automation and switch to YAML view so you can paste in the contents of the offline-motion-events/#Ring - driveway_latest_event Download.yaml file from this repo-being sure to change the alias (name) to match your automation name and then save it.

# Testing everything:

* At this point, and assuming you have at least one event waiting for you on Ring.com that you can see in the Ring app, you can test the automation and customizations:
1) In Home Assistant, head to settings>automations and find your new automation, click it and keep in the visual editor so you can see the actions
2) Execute the download action by clicking the verticle 3 dots to the right of the Downloader:Download file action
3) You should now see driveway_latest_event.mp4 in your HA config/www folder path, if you don't, check that your Ring camera has an event and then also check that your ring-mqtt-ha-addon is working before coming back here and trying the download again and run the next download to get the driveway_snapshot.jpg image too
4) Once you have the files you can test the next action which will update the driveway_timestamp input_text helper entity with the exact time
5) At this point your HA overview dashboard card you added above, should come to life with the same preview image that your Ring app shows for the latest motion event and if you click it, it should immediately play the video (muted) without any cloud delays

Enjoy! and please do ask questions, I may have forgotten a step as this was quite a journey for me which involved first realizing that the included Ring integration is no longer maintained and stops reporting motion sates after a short time of each HA restart and also tryhing several other dashboard card "tricks" to see if I could make it work with the Ring.com cloud links and then figuring out now to make the cards refresh.

# About (original)
This add-on provides users of Home Assistant OS or Home Assistant Supervised an easy method to install and run the [ring-mqtt](https://github.com/tsightler/ring-mqtt) project which allows various devices sold by Ring LLC to integrate easily with Home Assistant via the open MQTT protocol.  The project also supports video streaming by providing an RTSP gateway service that allows any media client supporting the RTSP protocol to connect to a Ring camera livestream or to play back recorded events (Ring Protect subscription required for event recording playback).  Please review the full list of [supported devices and features](https://github.com/tsightler/ring-mqtt/wiki#supported-devices-and-features) for more information on current capabilities.

**!!!! Important note regarding camera support !!!!**    
The ring-mqtt project does not turn Ring cameras into 24x7/continuous streaming CCTV cameras.  Ring cameras are designed to work with Ring cloud servers for on-demand streaming based on detected events (motion/ding) or interactive viewing, even when using ring-mqtt, all streaming still goes through Ring cloud servers and is not local.  Attempting to leverage this project for continuous streaming is not a supported use case and attempts to do so will almost certainly end in disappointment, this includes use with NVR tools like Frigate, Zoneminder or others and there are significant functional side effects to doing so, most notably loss of motion/ding events while streaming (Ring cameras only send alerts when they are not actively streaming/recording).  While you are of course welcome to use this project however you like, questions about use of such tools, or issues opened about these tools, will be locked and deleted.

## Support
If you need help with this addon please use the [discussions section](https://github.com/tsightler/ring-mqtt/discussions) on the main [ring-mqtt project](https://github.com/tsightler/ring-mqtt).

## Quick Install
This is a Home Assistant addon and must be added to the native Home Assistant add-on store, this project has nothing to do with HACS and attempts to add this repository to HACS will fail.  The Home Assistant add-on store is only available when running Home Assistant Supervised installed via either Home Assistant OS or manually.  If you are running Home Assistant Core via Docker or manual install into a Python virtual environment then there is no support for the addon store but you can still run the [ring-mqtt](https://github.com/tsightler/ring-mqtt) project directly to get the same capabilities.

**This add-on requires a working MQTT broker.  Configuring Home Assistant MQTT support is outside of the scope of this document but the standard Home Assistant Mosquitto integration along with the Mosquitto MQTT add-on is the recommended configuration.**

To install this addon follow these steps:

1) Navigate to the add-on store in the Home Assistant UI (**Supervisor** in the left menu, then **Add-on Store** on the top tab)
2) Select the three vertical dots in the upper right-hand corner and select repositories
3) In the **Manage add-on repositories** screen enter the URL for this projects Github page (https://github.com/tsightler/ring-mqtt-ha-addon) and click add
4) After adding the repository scroll to the bottom of the list of addons or use seach to find the addon
5) Select the addon and click the **Install**
6) Proceed to the [configuration documentation](DOCS.md)

Please refer to the [ring-mqtt project wiki](https://github.com/tsightler/ring-mqtt/wiki) for complete documentation on the various features and configuration options.
