---
title: Low Battery - Mate
author: meyer
date: 2023-03-11 02:24:00 -0500
categories: [Tutorial]
tags: [linux, parrot, mate, customization]
---

## Introduction

Do you have problems with low battery alerts?

## Create a custom alert

First we need to create a bash script:

```shell
sudo touch /usr/local/bin/battery_monitor.sh && 
sudo chmod +x /usr/local/bin/battery_monitor.sh
```

Then, copy and paste the following script to `/usr/local/bin/battery_monitor.sh`

```shell
#!/bin/bash
#
# Author: Meyer Pidiache (github.com/meyer-pidiache)
#

# Get battery level
BATTERY_LEVEL=$(cat /sys/class/power_supply/BAT0/capacity)

# Get status (On AC / On Battery)
STATUS=$(cat /sys/class/power_supply/BAT0/status)

# Low battery percentage
BATTERY_THRESHOLD=20

# Check low battery
if [ "$STATUS" == "Discharging" ] && [ $(cat /sys/class/power_supply/BAT0/capacity) -le $BATTERY_THRESHOLD ]; then
  # Don't forget to change "username"
  DISPLAY=:0 XAUTHORITY=/home/{yourusername}/.Xauthority /usr/bin/notify-send --urgency=critical --icon=dialog-warning "Low battery ($BATTERY_LEVEL%)."
fi
```

> Edit the script with the correct path to `~/.Xauthority`
{: .prompt-warning }

### Crontab

Finally, add the path script to crontab

```shell
crontab -e
```

Add this line at the end of the file:

```shell
# m h dom mon dow   command
*/5 * * * * /usr/local/bin/battery_monitor.sh
```

This will check the battery level every 5 minutes, and if it's less than or equal to 20, the alert will be shown.

> In `*/5 * * * *`, the `*` symbols represent the different time intervals, with the order being `minute hour day-of-month month day-of-week`.
{: .prompt-info }