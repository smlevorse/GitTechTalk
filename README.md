# GitTechTalk
Resources for a Git tech talk for RIT's Society of Software Engineers

## Git What?

[Git](https://www.git-scm.com/) is a **Distributed Version Control** system. It runs across multiple devices on many platforms to allow teams and individuals to collaborate and track changes in a project. Typically, Git is used for programming poroject, but it works well with any text file based projects. Git was created by Linus Torvalds in 2005 who created it to help with maintaining Linux([Source](https://www.ted.com/talks/linus_torvalds_the_mind_behind_linux/transcript?language=en#t-519630)). Today, Git has grown into one of the most widely used version control systems with heavy influence from the Free and Open Source Software movement. 

## Getting Git

For Mac and Windows systems, you can find installers for Git on their [Downloads page](https://git-scm.com/downloads)

For Linux/Unix systems, there is a Git package for almost every distro. Find details about how to install for your distro [here](https://git-scm.com/download/linux).

## Git Config
![GIF of person adjusting knobs and dials](https://media.giphy.com/media/l0K3YaSLgPZ9QSnyU/giphy.gif)

After installing Git, you may want to customize your git environment. Git maintains two configurations that control how it behaves, a `global` config for your development environment, and a `local` config that dictates one local repository on your computer. The local config only applies to your environment. Multiple people working in one repository will have different configs.

Git keeps track of your identity so that your work can be marked as contributed by you. To configure how Git refers to you, set the following configs:

```
git config --global user.name="Git Guru"
git config --global user.email="guru@git-scm.com"
```

## Setting up a repository

A **repository** is a central location in which data is stored and managed [[OED 1.2](https://en.oxforddictionaries.com/definition/repository)]

A repository is Git's root organizational structure for maintaining a project. It contains all of the files for the project, as well as information about the project's contributors, configuration, and change history. To create a repository on your computer, first set your working directory to be the folder you want to make a repo and run `git init`
```
mkdir myNewRepo
cd myNewRepo
git init
```
([init docs](https://git-scm.com/docs/git-init))

You'll notice that this creates a `.git` folder where you ran the command. This folder contains all of the information Git needs to work its magic. 

