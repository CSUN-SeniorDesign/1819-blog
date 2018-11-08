---
title: "Project 3 Post 1: Daniel"
author: "Daniel Hirahoka"
cover: "/img/cover.jpg"
tags: ["Daniel Hirahoka", "Zephyr", "Week 7"]
date: 2018-10-12T16:20:13-07:00
draft: false
---
For Project 3 I tasked myself with doing the Docker fIle. From my understanding this will essentially do what Packer does in regards to our image but with a more efficient and more compressed program called Docker. Since I'm not really familiar with Docker I had to read through the documentation to understand how I would begin the task of creating a image with Docker.

To structure my task I had to understand what needed to be in the container. The project 3 instructions states that Hugo and Nginx have to be in the Docker file to make our site available. To begin I created to Docker Files that would run alongside each other. I thought this was the way it had to be done because for some reason when I was researching I had read that running multiple FROM lines in a single Docker file was bad. So I created two directories that would contain my containers for each of the requirements.

One container contained the Nginx image and the other contained the Hugo image:

	mkdir hugodevelopment && mkdir nginx

After creating the directories I had to focus on the individual container. Putting myself in the hugodevelopment directory I created a Docker File. Looking on the Docker hub for public images that would help me with Hugo I found 2 that seemed to do what I wanted. It would install Hugo and then grab the files from a directory called site and build the site using Hugo on localhost:1313. Building this image and running it seemed to have worked the only issue I ran into was that I wasn't able to see my blog on the localhost but using "EXPOSE 80" that would open up port 80 somehow fixed this issue, wasn’t sure why. Afterwards I had my blog in the same directory in a folder called site and going to localhost:1313 I would see it running perfectly.

Docker File for Hugo:

	FROM monachus/hugo
	EXPOSE 80

After I would now work on the Nginx requirement which was very similar to the previous Docker File. It was straight forward, change my work directory to Nginx and then create the Docker File that referenced an Nginx base image. Looking on the Docker Hub I found the Official Nginx repository that I could choose which Nginx version I would like. I chose 1.15.10-Alpine instead of just Alpine. The reason for this was that Alpine would always choose the must up-to-date Nginx and if they released something that didn't work or was compatible with Hugo it could break. I chose a more stable version of Nginx that If we needed to upgrade I could do so personally.

Docker File for Nginx:

	FROM nginx:1.15.5-alpine
	EXPOSE 80

Now most of the issues came from Nginx and trying to get it work with the other container. Since I was unfamiliar with how Docker works I was unsure on how to get both to work together. I learned about Volume images and even on the Hugo repository in Docker they suggested that if I wanted to I could use the Hugo image as a volume image for Nginx. I used the suggested command that Hugo gave but it just wouldn't really give me anything as an output.

Volume Image commands:

	docker run -d -v /usr/share/nginx/html --name site-data my/image
	docker run -d --volumes-from site-data --name site-server -p 80:80 nginx

Reading it now I'm not to sure what these commands are meant to do. My understanding is it would take what Hugo built and then place it into the html folder that was found in the Nginx image. Afterwards it would then run Nginx on port 80 where we would be able to see it. I tried localhost:1313 and localhost:80 but nothing showed up. I concluded that it might have to do with the config file that Nginx is referring to, I would have to edit it that file to see what exactly it was doing and what it should be doing.To make sure that this was how it's suppose to be done I had asked the professor for some advice on how it should be done, if using volume containers was even the right way. He suggested Multi-stage containers which I have never heard of but essentially disproved what I read earlier about the multiple FROM lines in a Docker File. Essentially Multi-staged container was going to do what two Docker Files did in one, build Hugo and immediately build the Nginx. This had two major benefits, I didn’t have to run two containers obviously and everything was dealt in the same container. So when I wanted to edit, reference or create anything both images would easily be able to refer to them. 

Current Multi-Stage Docker File:

~~~
FROM alpine:3.5 as build

ENV HUGO_VERSION 0.49.1
ENV HUGO_BINARY hugo_${HUGO_VERSION}_Linux-64bit.tar.gz

#Install Hugo
RUN set -x && \
	apk add --update wget ca-certificates && \
	wget https://github.com/spf13/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY} && \
	tar xzf ${HUGO_BINARY} && \
	rm -r ${HUGO_BINARY} && \
	mv hugo /usr/bin && \
	apk del wget ca-certificates && \
	rm /var/cache/apk/*

COPY ./zephyrblog/* /site/

WORKDIR /site

RUN /usr/bin/hugo

FROM nginx:1.15.5-alpine

COPY ./conf/default.conf /etc/nginx/conf.d/default.conf

COPY --from=build /site/public /var/www/site

WORKDIR /var/www/site
~~~

The issue I'm currently running into is that Hugo fails to complete the build because it can't seem to find the theme directory. I'm working with my team to see how to fix the issue.


