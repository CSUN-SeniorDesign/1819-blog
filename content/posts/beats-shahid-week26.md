---
title: "Shahid's Blog. Week 26."
date: 2018-4-12T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 26"]
layout: 'posts'
---

## Shahid's Blog. Week 26
#### What's new this week?
This week I started work on a common automation framework. This will allow any website to be tested with one framework rather than having to create a new framework for every website that's to be tested.

# CommonAutomationFramework
An automation framework built with TestNG and Selenium to automate the testing of websites and web applications.

# Setting up an Automation Framework with Selenium and TestNG

## Setting up the Project with Maven in IntelliJ IDEA
The project will be managed with the software project management tool: Maven.

1. Start a Maven project by clicking on Create New Project.

2. Select the Maven tab then click Next.
3. Give a GroupID and an ArtifactID then click Next.
4. Give a Project Name and Location then click Finish.

## Adding Selenium, TestNG, and Other Tools to the Maven Project
1. Go to https://mvnrepository.com/
2. Type Selenium into the search bar and click on Selenium Java.
3. Click on version 3.141.x then copy the contents of the Maven tab to your clipboard.
4. In the Maven project's pom.xml file create a "dependencies" tag
5. Paste the contents of your clipboard inside the "dependencies" tag.
    ```
        <dependencies>
            <dependency>
                ...
            </dependency>
        </dependencies>
    </project>
    ```
6. Type TestNG into the search bar and click on Selenium Java.
7. Click on version 7.0.0-beta3 then copy the contents of the Maven tab to your clipboard.
8. Paste the contents of your clipboard inside the "dependencies" tag.
9. Type WebDriverManager into the search bar and click on WebDriverManger by io.github.bonigarcia
10. Click on version 3.4.0 then copy the contents of the Maven tab to your clipboard.
11. Paste the contents of your clipboard inside the "dependencies" tag.
12. When an artifact added to the Maven project you have to import the changes so either click on "Import Changes" everytime the popup appears or "Enable Auto-Import" just once to automatically import any changes.
