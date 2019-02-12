# GitTechTalk

Resources for a Git tech talk for RIT's Society of Software Engineers. Big thanks to [@kahliloppenheimer](https://github.com/kahliloppenheimer) for teaching me advanced Git, and Rachel Carmena for creating [How to Teach Git](https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html). 

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

You'll notice that this creates a `.git` folder where you ran the command. This folder contains all of the information Git needs to work its magic. For most cases, you won't need to modify this folder at all. Instead, all work should be done in the same directory that contains the `.git` directory. 

## Remotes
![Rex from Toy Story clicking a TV Remote](https://media.giphy.com/media/zlLydol7ndM7C/giphy.gif)

A **remote** is a repository that exists outside of your development environment. Typically, remotes are hosted on a dedicated Git server. If you use a service such as GitHub, BitBucket, or GitLab, they create a remote repository for you with the nececssary configurtions for remote development. You can also create a remote repository on your own server by running the command `git init --bare`. 
![A diagram demonstrating Remote repositories exist on a separate server](https://rachelcarmena.github.io/img/cards/posts/how-to-teach-Git/general-drawing.png)

You can add a remote for an existing local repository with the command `git remote add`
```
git remote add name url
```
where `name` is the name you can refer to this remote locally and `url` is where the remote resides. 

Incidentally, this is how the concept of Forking works. For example, if you wanted to Fork this repository without using GitHub's Fork button, you would use the following steps:

1. Create a new repository on GitHub
2. Clone this repo to your local machine
```
git clone https://github.com/smlevorse/GitTechTalk.git
```
3. Add a remote pointing to your repository
```
git remote add myFork https://github.com/username/forkRepo.git
```
4. Push the code up to your new fork
```
git push -u myFork master
```

## Clones

![A diagram demonstrating how cloning works](https://raw.githubusercontent.com/rachelcarmena/how-to-teach/master/git/clone.png)

In Git, **cloning** is the act of copying a repository from some _remote_ location to a local dev environment. When you run `git clone`, it creates a new copy of the repo, that is, you do not need to run `git init` before cloning. There are two methods for cloning a repo, depending on which protocol you want to use:

Over https:
```
git clone https://github.com/smlevorse/GitTechTalk.git
```

Over SSH:
```
git clone git@github.com:smlevorse/GitTechTalk.git
```

Typically https is easier to set up, but some companies may require you to use SSH.

After cloning a repo, a remote named `origin` is created by default that points to wherever you cloned from. 

## Standard Workflow

Most of the time, when using Git, you stick to the "Standard Workflow." That is, the mantra that most people use to teach Git, "Add, Commit, Push." Git allows for collaboration, but it doesn't have any sort of autosave functionality like Google Docs or Office 365 do. Instead, you must first tell Git which files to save (Add), then tell Git when you want to save (Commit), and finally share the saved changes with the rest of your collaborators (Push). The result is a the typical workflow that has been drilled into many developers' heads for years. 

![A diagram explainging how changes flow through the standard workflow](https://raw.githubusercontent.com/rachelcarmena/how-to-teach/master/git/add-commit-push.png)

To understand how git actually works throughout this workflow, you need to understand the four locations where git operates:

* Working Directory: This is the directory and subdirectory that is on your filesystem. This is where you do work. Git is unaware of changes you make here until you tell it to check for changes.
* Staging Area: After making changes to files in the working directory, you tell Git to move those changes to the staging area. This acts as a place to organize what changes will and won't be used in the project.
* Local Repository: This is the repository where changes are tracked. Changes from staging get packaged up and sent here, maintaining a local version history.
* Remote Repository: Discussed above, this is where changes are maintained for the entire project. 

### Add

Copies changes from the working directory to the staging area.

```
git add file.java
```

This command supports [glob patterns](https://github.com/begin/globbing). That is * and ** act as wildcards:

```
git add *.java
```

You can also use the `-A` flag to stage all changes

```
git add -A
```

After piecing together which files have changes that you want to commit, you can run `git status` to see a summary of any unstaged changes, or which changes have already been staged.

### Commit

Once you have all of the changes that you want staged, you'll want to package them up so they can be referred to for future reference. This is done with the `commit` command. A **commit** is a snapshot of the state of the repository at a given time. Commits contain information including:

* What the difference between files from this commit and the last commit
* Who made those changes
* When the changes were committed
* A message describing what the changes are or why they were made
* A SHA hash acting as a unique identifier
* A reference to preview commits in the graph\*

To create a commit, run:
```
git commit -m "A short meaningful description"
```

This packages up all of the changes staged in the staging area and updates the local repository with the new commit. After the commit is made, the local repository's history is updated to include the new commit. To see the history, run `git log`:
```
commit b5c9987185f5cff24010a5fdf230295e7589ba9d
Author: Sean Levorse <smlevorse@msn.com>
Date:   Thu Feb 7 11:44:46 2019 -0500

    Initial commit
```
By default, you'll see a list of commits including the SHA, the Author, the time the commit was made, and the commit's message. You can use the up and down arrows to navigate through the log. Press `q` to quit and return to the command line. The log has many useful flags for obtaining information about commit history. Check out the [docs](https://git-scm.com/docs/git-log) to see some of the powerful ways to leverage this tool. 

>\*Commits are often visualized as nodes in a directed graph. Each commit points to one or more commits that typically lead back to the first commit in a repository. Visualizing commits this way makes it easy to see how project changes branch out and come together. 

### HEAD

HEAD is a concept you will see come up frequently when working with Git, especially if you seek out help on sites like StackOverflow. To put it simply, HEAD always refers to the newest commit on the current branch. People often call it a "pointer to the tip of the current branch."

HEAD is a [reference](https://git-scm.com/book/en/v2/Git-Internals-Git-References), and there are several other similar refernces, such as MERGE_HEAD that are relvant. But HEAD is the most common and it's worth mentioning here. 

### Push

Now that your changes have been made and committed to the local repository, you'll want to tell the remote repository about your changes so you can share them with your collaborators. This is done through the `push` command:

```
git push origin master
```

When pushing, you usually need to specify which remote and which branch to push to. If you want to set a default remote/branch for a local branch to push to, you can use the `-u` flag:
```
git push -u myFork master
```

**`git push` will fail if the remote repository's history differs from the local repository's history in a way that doesn't make sense.**

For the most part, if a push fails, it means that someone else has pushed changes while you were working. To fix this, see Fetch and Push below. There are a handful of cases where a force push is necessary. However, a force push is quite dangerous and can cause you to make unfixable changes resulting in the loss of work. Force pushes should be used sparingly, done with care, and by convention are NEVER done on the master branch. Force pushing is telling git "I know you think the history should look like this, but you're wrong, my history is the truth, use this instead." To force push, use:
```
git push -f
```

### Reset and Revert

Mistakes happen. Bugs pop up. Experiments fail. To err is to human. But fortunately Git has your back. There are two useful commands that leverage the version control aspects of Git to rectify mistakes in the project. 

#### Reset

Git [reset](https://git-scm.com/docs/git-reset) sets files in your working directory to the status they were at a given point in time. By default, `git reset` resets the working directory to HEAD. 

```
git reset --hard
```
will undo any local changes. The `--hard` tells git it can overwrite the working directory. 

```
git reset --hard eac1380
```
You can tell Git to reset the working directory to the state of a specific commit. Note, if you try to push from this point, the push will fail because the remote has commits the local repository does not. 

You can also reset individual files.
```
git reset --hard eac1380 README.md
```

#### Revert



## Fetch

## Pull

## .gitignore

## Branching

### Checkout

### Merging

### Merge Conflicts

### Rebasing

#### Rebase Interactive

## Other Cool Git Commands

## Cherry Pick

## Further Learning

## Useful Links
