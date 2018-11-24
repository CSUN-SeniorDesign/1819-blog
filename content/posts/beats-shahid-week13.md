---
title: "Shahid's Blog. Week 13."
date: 2018-11-23T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 13"]
layout: 'posts'
---

## Shahid's Blog. Week 13
#### What's new this week?
I had to create documentation on how to upload project files to GitHub for the COMP SCI side of the Irrigation group.

## Uploading to GitHub

## Table of Contents
- Section 1: Installation

- Section 2: First time setup

- Section 3: Updating the repository

### Section 1: Installation

#### Installing a GUI for GIT

tl;dr: Keep all defaults and click next through everything.

1. Visit ```https://git-scm.com/downloads``` and download the installer for your operating system.
2. Once downloaded, run the installer.
3. At the license, click next.

4. Leave the install location as the default and click next.  

5. Leave the components as the default and click next.

6. Leave the start menu folder as the default and click next.

7. Leave the default editor as the default and click next.

8. Leave the PATH environment as the default and click next.

9. Leave the SSH executable as the default and click next.

10. Leave the HTTPS transport backend as the default and click next.

11. Leave the line ending conversions as default and click next.

12. Leave the terminal emulator as default and click next.

13. Leave the extra options as default and click next.

14. Leave the experimental options as default and click install.

### Section 2: First time setup

#### Adding a repository to your local machine (Cloning)

There are two methods to clone a GitHub repository to your local machine.

Note: This only needs to be done once.

##### Method 1: With the GUI

1. Open an explorer to the location where you want to create your local copy of the GitHub repository.

2. Right click on the whitespace and click ```Git GUI Here```

3. Click ```Clone Existing Repository```

4. For the source location copy and paste the "top level" of the GitHub repository ```https://github.com/CSUN-Irrigation/software-irrigation```
5. For the target directory copy and paste the location where you want the repository to be copied to on your local machine. Mine was ```D:\Work\Google Drive\CSUN\Fall 2018\CIT 480\CIT 480 Final Project\software-irrigation```

  Note: Unlike the method using CLI, here you should include the folder name that's the same as the repository name. (software-irrigation)
6. Press ```Clone```

##### Method 2: With CLI

1. Open an explorer to the location where you want to create your local copy of the GitHub repository.

2. Right click on the whitespace and click ```Git Bash Here```

3. Type ```git clone https://github.com/CSUN-Irrigation/software-irrigation``` and press enter.

  Hint:

  ```
  You can right click and press paste to paste long links.
  ```

  Warning:

  ```
  You have to give the link to the "top level" of a GitHub repository.
  ```

  Example:

  ```
  ```https://github.com/CSUN-Irrigation/software-irrigation``` is acceptable.

  ```https://github.com/CSUN-Irrigation/software-irrigation/tree/master/app``` is NOT acceptable.
  ```

4. The repository with the name ```software-irrigation``` should now be a folder in the directory you right clicked and opened the git CLI from.


### Section 3: Updating the repository
Ideally, you'll want to save your work to GitHub once the day is over or you've finished working on it for that day.

tl;dr: Right click in the repository directory whitespace, open ```Git GUI Here```, press rescan, press ```Stage Changed```, Add a ```Commit Message```, create a new ```Branch```, press ```Commit```, press ```push```, open a brower to the github page, press ```Compare and pull request```, have team member check and accept the pull request.   

1. Open up the folder containing the GitHub repository.

2. Right click and select ```Git GUI Here```


3. Once the GUI opens, press ```Rescan``` so that the program checks for any changes to the files inside the directory.

4. Then click on ```Stage Changed```. This will add the changed files to the tracker that git uses to check for any changes to the files inside the directory.

5. If you get this message, just click ```Continue```

6. In the ```Commit Message``` box, the first line should be a summary of all the changes you are making to the repository. Then leave a blank line below the summary and add a detailed account of what was changed. The more specific the better.

7. Never commit your changes to the master branch. Always create a new branch to push to first.

8. Name the branch something descriptive.

9. After the branch has been created, it'll return you to the main GUI. From there press ```push```. Select the newly created branch.

10. Visit the GitHub repository on your browser. Click the ```format-change Compare & pull request``` option below the blue line.

11. Write any necessary comments for your team members such as "please check [this specific part of my code] please."

12. Have one of your team members check over the changes made to the project. Once the check is complete, press ```Merge pull request```. The repository will be updated with the changes you've committed to it.

Explanation: Committing to the repository is done in this way so that you have a log of all your contributions to the team project. It also gives an opportunity for your team to check your work and for you to have some peer review.
