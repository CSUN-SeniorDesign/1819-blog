---
title: "Shahid's Blog. Week 17."
date: 2018-2-01T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 17"]
layout: 'posts'
---

## Shahid's Blog. Week 17
#### What's new this week?

## Selenium WebDriver
Since the software-irrigation group is making a website for us to host, I thought it would be a great idea if we could also handle debugging and asserting for their website. We would make a framework where we would test to see if their website is working properly. For example, going to the website URL will return the expected page title, then go to the login page via clicking on the login link. Again we would assert that the expected login page title is equal to the actual login page title and see if all the UI works properly on the website. Because of the push for the "Shift Left" methodology where the industry is pushing developers rather than quality assurance testers towards using debugging frameworks, it's important for DevOPS to be able to do the same.

## Setting up Selenium WebDriver with Eclipse and Maven in Java
1. After downloading, installing, and opening Eclipse, go to File > New > Project > Maven Project.
2. Click Next, then select Create a simple project (skip archetype selection)
3. Specify the GroupID, ArtifactID, Name, and Description.
4. Click Finish to create the project.
5. Open up pom.xml and add the following:
  ```
  <properties>
    <java.version>1.8</java.version>
  </properties>
  ```
  The above specifies that the current project is running on java version 1.8

6. Next go onto your browser and go to https://mvnrepository.com/
7. Find JUnit, Maven Surefire Plugin, Hamcrest-all, Selenium-Java, and webdrivermanager.
8. Copy each of the provided dependancy declarations found on the pages listed above. They will look like this:
  ```
    <!-- https://mvnrepository.com/artifact/junit/junit -->
  	<dependency>
  	    <groupId>junit</groupId>
  	    <artifactId>junit</artifactId>
  	    <version>4.12</version>
  	    <scope>test</scope>
  	</dependency>
  ```
  The above tells Maven to go and download the dependency files required to use the features from each dependency. Once all the dependencies are all downloading, the project can be started.
