---
title: "Second Post - Tonny Wong"
author: "Author Name"
cover: "/img/cover.jpg"
tags: ["Tonny Wong", "Zephyr", "Week 2"]
date: 2018-09-05T18:41:01Z
draft: false
---

<br>

For the second week of project 0, I have found out that the Hugo server will be held in Ubuntu. So I installed Hugo for the Ubuntu instance. The first attempt to move the files from my Windows version of Hugo to Ubuntu is through the zip file and proceed to unzip the files in the site. When I tried to run the Hugo server it seems that there was errors all over the theme files and did not run the Hugo server properly. What happened is that i deleted the files and proceeded to do the zip and unzipping again and again it did not work. So i tried creating the Hugo Server from Ubuntu itself, basically from scratch. Since I utilized cmder over Windows I did not required to learn much about different code lines other than a different method in approaching themes. Instead of having the config.toml pre-setted i have only added “theme=hugo-lamp” in order to grab the theme which was placed in the theme folder. Another change of command was opening the site through Ubuntu and nginx which was completely different from just using “hugo server” which was instead used as “Hugo server --bind=0.0.0.0 --baseURL= < URL with IP Address> / -D -F” -D meant for draft purposes so that the original files do not get touch before confirmation. Then after knowing that the blog is working from the Web server. We then proceeded to create the documentation and tutorials sort to speak of a “how to” (tutorial).
