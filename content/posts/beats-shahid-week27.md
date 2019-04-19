---
title: "Shahid's Blog. Week 27."
date: 2018-4-19T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 27"]
layout: 'posts'
---

## Shahid's Blog. Week 27
#### What's new this week?
This week I converted my school/work laptop from a windows machine to a linux one. In the process of using Linux as a daily driver rather than strictly for schoolwork, I found a lot of interesting things that linux does that goes unnoticed when only using Linux in a VM. 

The most important; however, has been adding a program to your /usr/bin folder allows you to call the executeable for that program from any directory through the terminal.

For example:

If you were to download Intellij IDEA, you would get a tar file. You could either right click and select "Extract here" or you could use the command line and run the following:
```
tar -xvf yourfile.tar
```

Then you can move the folder into your /usr/bin folder and create an executeable file with the name of the command you want to run in order to start the program.

```
sudo nano idea

#! /bin/bash
/usr/bin/intellij-idea/Idea

sudo chmod +x idea
```

Finally you can go into any directory and type "idea" and Intellij IDEA will start up. 