---
title: Mate - Low Battery Custom Alert
author: meyer
date: 2023-03-11 02:24:00 -0500
categories: [Tutorial]
tags: [linux, parrot, mate, customization]
---

## Introduction

Do you have problems with low battery alerts? With this script you can create a custom low battery alert, with a few steps you can do it so, let's start.

## Preview

To see a preview of this custom alert excute:

```shell
notify-send --urgency=critical \
            --icon=dialog-warning \
            "Low battery $(cat /sys/class/power_supply/BAT0/capacity)%."
```

> Note that `--icon` use a default icon, you can replace it with a image path
{: .prompt-info }

## Create a custom alert

First we need to create a bash script

```shell
sudo touch /usr/local/bin/battery_monitor.sh && 
sudo chmod +x /usr/local/bin/battery_monitor.sh
```

Then, copy and paste the following script to `/usr/local/bin/battery_monitor.sh` and edit if necesary.

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
{: file='/usr/local/bin/battery_monitor.sh'}

> Edit the script with the correct path to `~/.Xauthority`
{: .prompt-warning }

### Crontab

Finally, add the path script to crontab

```shell
crontab -e
```

Add this line at the end of the file:

```conf
# m h dom mon dow   command
*/1 * * * * /usr/local/bin/battery_monitor.sh
```
{: file='crontab -e'}

This will check the battery level every minute, and if it's less than or equal to 20, the alert will be shown.

> In `*/1 * * * *`, the `*` symbols represent the different time intervals, with the order being `minute hour day-of-month month day-of-week`.
{: .prompt-info }
