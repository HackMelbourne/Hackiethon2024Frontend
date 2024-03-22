# Git Tutorial 

Note that this assumes you have python and a code editor (Eg. vscode downloaded) 

## Downloading Git 
First you will need to download Git onto your computer... 
You can do so through this [Git Download Link](https://git-scm.com/downloads)

## Configuring Git 
After installing Git onto your computer, you will need to configure it with your name and email address
```
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```
Replace "Your Name" with your name and "your_email@example.com" with your email address.

## Initializing a Git Repository
To start version controlling your project, navigate to your project's directory (folder) using the terminal and run:
```
git init
```
This creates a new Git repository in your project directory.

## Adding files to your Repository

To add files to the staging area (the area where changes to files are prepared to be committed), you can use:
```
git add filename
```
Or if you want to add all of them at once, you can use:
```
git add .
```
## Committing a change to the repository 
Once you have added the files, you need to commit them to "save them' 
```
git commit -m "Your commit message here"
```
Your message should be a brief description of the changes you have made to the project since the last commit.


## Checking Status / logs in Git 
Git has a command to check the status of your files, you can do so with the following: 
```
git status
```
This will show you which files are modified, which files are staged for commit, and which files are untracked.

If you want to check the a list of the commits that have been used, you can use:
```
git log
```




