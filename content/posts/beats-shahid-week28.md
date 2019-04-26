---
title: "Shahid's Blog. Week 28."
date: 2018-4-25T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 28"]
layout: 'posts'
---

## Shahid's Blog. Week 28
#### What's new this week?
#### Working with Git
This week I had to learn how to fork a repo and sync content between the fork and the original repo. 

### Notes
Original Repo is: https://github.com/CSUN-Irrigation/testrepo
Forked Repo is: https://github.com/h1ddengames/testrepo

#### Forking a repo
1. In order to fork a repo, go to the git(hub, lab, etc) website where the repo is located. 
2. There will be three buttons "Watch", "Star", and "Fork". Click on the Fork button. This will give you the option to create a copy of the repo on any organization you're a member of or your own account.

#### Using Git with the original repo as your local
1. Clone the original repo.
```
git clone https://github.com/CSUN-Irrigation/testrepo
```
This copies the files from the website to your local machine.

2. Make changes to your project.
```
echo "this is a new file" > file1
echo "this is a second new file" > file2
```

This overwrites the contents of the file with the sentence "this is a..." to the file with the filename listed. 

3. Stage changes to the project and commit these changes.
```
git add file1
git add file2
git commit -m "Changed files 1 and 2" -m "Added a sentence describing the changes"
```

This adds each file individually to the list of files that will be updated on the website. 

The first quote will be the message while the second quote will be the description.

OR 
```
git add .
git commit -m "Added 2 files" -m "Added files with file descriptions inside the file itself.
```

This adds each file in the directory to the list of files that will be updated on the website.

4. Create a branch for this new feature.
```
git branch newFeature
```

Each branch should be a new feature that is being implemented. We are creating a branch because you should NEVER push to master. <b>All feature branches should be peer reviewed then pulled into the master branch.</b>

5. Switch to the new branch. 
```
git checkout newFeature
```

This will switch our branch from master to newFeature.

6. Once the changes are finalized for the current branch, push the changes to the branch on the website.
```
git push origin newFeature
```

7. Go to the website, then click on "Compare and Pull" then confirm on the next screen.

8. Peer review your work. Once it's acceptable, have them accept your pull request. This can be done on the website or using the commandline which is given by the instructions on the website. Update your repo once master branch has been updated.
```
git pull origin master
```

9. Update your remote to the forked repo.
```
git remote set-url origin https://github.com/h1ddengames/testrepo.git
```

10. Verify that the remote origin has changed.
```
git remote -v
```

11. Push to the forked repo.
```
git push origin newFeature
```

12. Do steps 7 and 8 yourself since this is your forked repo. 


#### Using Git with the forked repo as your local

Only the steps that are different from the section "Using Git with the original repo as your local" will be listed here.

0. Set the upstream repo to the original repo.
```
git remote add upstream https://github.com/CSUN-Irrigation/testrepo
```

1. Clone the forked repo.
```
git clone https://github.com/h1ddengames/testrepo
```

Steps 2 - 8 are the exact same for this section. <b>Note: since this is your forked repo, you should review your changes yourself in your own pull request. Once it seems correct, then pull the changes to master.</b>


3. Update your remote to the original repo.
```
git remote set-url origin https://github.com/CSUN-Irrigation/testrepo.git
```

Step 10 and 11 are the exact same.

4. Reset your remote to the forked repo.
```
git remote set-url origin https://github.com/h1ddengames/testrepo.git
```

5. Update your forked repo.
```
git checkout master

git pull --rebase upstream master

git pull origin master
```