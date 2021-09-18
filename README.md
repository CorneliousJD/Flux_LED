# About
Modifies homeassistant/components/flux_led/light.py to fix issue where LED lightstrip using MagicHome controller lights turn off immediately after turning them on. Based on modification by john32 with some additional files and instructions. See https://community.home-assistant.io/t/flux-led-magiclight-dont-work-since-few-updated/145179/5

This is designed to specifically fix v8 firwware MagicHome Flux LED WiFi RGB controllers, I do NOT have RGBW or RGBCW strips or controllers ot test with, so your mileage may vary if you have other controllers or other firmware than v8.

# Usage

### HACS installation
This can be installed via HACS! To do this, follow these instructions: 
  1. Copy the URL to this respository: https://github.com/RandomArray/Flux_LED/
  2. In HACS, select integrations
  3. Click the 3 vertical dots (⋮) at the top right of the HACS integration page
  4. Select 'Custom repositories'
  5. Paste this repositories in the 'Add custom repository URL' field, select the integration category, then click add
  6. The integration will now show as a new installable repository in HACS

### Manual installation 
You will need to put these files into your HomeAssistant /config/custom_components/flux_led/ folder.
After that, if you are running in a docker container you will need to make sure you enter the container and run 

```
cd /config/custom_components/flux_led

chown root:root
```


This should give the files the proper permissions needed for HomeAssistant Core to access the files.
Go ahead and restart HomeAssistant and then go into Developer Tools, then Log, and you should see

*WARNING (MainThread) [homeassistant.loader] You are using a custom integration for flux_led which has not been tested by Home Assistant. This component might cause stability problems, be sure to disable it if you experience issues with Home Assistant.*

Once you see that, you will know it's loading the new custom component you added. Go ahead and test from here, lights should now turn on/off, note that there is more of a delay than normal with this fix, it takes a second or two in order for it ot update it's state when you request a change, but working slowly is better than not working at all!

*As a workaround for the slow-ish polling, you can set `scan_interval` in the configuration.yml to a pretty low value. This makes it much snappier.*

Example:
```
  - platform: flux_led
    scan_interval: 0.5
    devices:
            10.0.2.4:
                    name: LED TV
                    mode: rgb
```

Thanks go to john32 on HomeAssistant Forums, and @skylord123 on GitHub for pointing me in the right direction here.
