---
title: "Shahid's Blog. Week 11."
date: 2018-11-08T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 11"]
layout: 'posts'
---

## Shahid's Blog. Week 11
#### What's new this week?
Continuing with the theme of my recent blogs, I've been working on my home server. I realized this week that TeamViewer is good for the times I'm away from home but still want a graphical interface however, I realized that I would need a failsafe. So this week I've set up and tried using Chrome Remote Desktop for Ubuntu 18.10.

## Setting up Chrome Remote Desktop

1. Install Google Chrome
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && \
sudo dpkg -i google-chrome-stable_current_amd64.deb
```

2. Sign into chrome.

3. Visit the chrome store and download Chrome Remote Desktop.

4. Install the Chrome Remote Desktop using the deb package.
```
sudo dpkg -i ~/Downloads/chrome-remote-desktop_current_amd64.deb && \
sudo apt-get install -f
```

5. Add the current user to the chrome-remote-desktop group.
```
sudo usermod -a -G chrome-remote-desktop $USER
sudo reboot
```

6. Stop and start the service with these commands:
```
  /opt/google/chrome-remote-desktop/chrome-remote-desktop --stop

  /opt/google/chrome-remote-desktop/chrome-remote-desktop --start
```
