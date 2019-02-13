# Demo

This is the demo that I will be following while giving the talk. This demo assumes that you already have Git installed and configured. Checkout the [README](../README.md) for this repo for detail on how to do that. Also be sure to have a text editor that you are comfortable with. This could be notepad, vim, nano, etc. 

## Set up a repo and fork this one

Create your remote repository. This can either be done locally with 
```
git init --bare
```

or can be done through the git hosting service of your choice.
* [GitHub](https://help.github.com/articles/create-a-repo/)
* [GitLab](https://docs.gitlab.com/ee/gitlab-basics/create-project.html)
* [BitBucket](https://confluence.atlassian.com/bitbucket/create-a-git-repository-759857290.html)

Now we're going to fork this repository. To do this run:
```
git clone https://github.com/smlevorse/GitTechTalk.git
cd GitTechTalk
git remote add myFork <<URL_TO_YOUR_ROMOTE>>
git push -u myFork master
```
The `-u` flag is important here. This sets the default remote branch to your fork instead of the origin repo.

## Making and committing changes

Change directory into the Demo folder
```
cd Demo
```

Open up `rit.md` and fix the branding. Specifically, change the colors from `Orange and Brown` to `Orange and White`:
```
 15 Enrollment | 19,000
 16 Colors | Orange and White
 17 Mascot | Ritchie
```

Save your changes and check our status:
```
git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   rit.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Notice how it says "`Changes not staged for commit`". We'll want to stage these changes:
```
git add rit.md
git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   rit.md
```

Now that our changes are staged, let's package them up into a commit:
```
git commit -m "Updated branding"
[master b0fa8e2] Updated branding
 1 file changed, 1 insertion(+), 1 deletion(-)
```

You can see the results of the commit with `status`
```
git status
On branch master
Your branch is ahead of 'myFork/master' by 1 commits.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean
```

Notice how it says "`Your branch is ahead of 'myFork/master' by 1 commits.'" This means that the history in your local repository has  one more commit than your remote, `myFork`.

Check the log to see the current history
```
git log
commit b0fa8e27f2aaa172328ef1f3682ec3a94f576924
Author: Sean Levorse <smlevorse@msn.com>
Date:   Wed Feb 13 10:37:22 2019 -0500

    Updated branding
:
```
The log contains the commit SHA, the author, the timestamp, and the commit message. This is useful for understanding how the repository changed and who changed it. 

Press `Q` to exit the log. 

Now we want to upload our new commit to the remote repository. Do this with `push`
```
git push
```
Since earlier when setting up this fork, we used the `-u` flag earlier, `git push` defaults to `myFork/master`

## Undoing mistakes

Open up `rit.md`. Delete a couple of lines and save your chages and close. Oops, you deleted something you shouldn't have. Let's undo that:
```
git reset --hard HEAD
```

Now open `rit.md` again and look at the lines you deleted. Delete a couple of more lines, and this time commit the changes.
```
git add rit.md
git commit -m "Deleted some lines"
```

Now you've done it. The changes are there in the repo, and need to be undone manually. Or can git help us?
```
git revert HEAD
git log 
```

Notice how your old commit `Deleted some lines` is still there with a new commit `Reverted Deleted some lines` on top. `revert` undoes the work for you without modifying the git history.

Push your changes to update the remote with the new history

```
git push
```

## Fetch and Pull

It's difficult to simultate changes in a remote with only one clone. If you would like to see how this works, create a second clone of the repo, this time from your remote

```
git clone <<YOUR_FORK_URL>>
cd GitTechTalk
```

Open up `Demo/rit.md` in this new clone and insert some stuff at the end of the file. Stage and commit these changes, and push them to your remote
```
git add -A
git commit -m "Added information"
git push origin master
```

Now go back to your original cloned directory and fetch the changes
```
git fetch myFork
```

If you check your status you'll notice that your local repo is behind in commits from the remote
```
git status
On branch master
Your branch is behind master by 1 commits.
 (use "git pull" to merge the remote branch into yours)
```

To take in these new commits, run
```
git pull myFork master
```
Now your repository is up to date. If you open `rit.md`, you'll notice the changes that were made in the other clone.

## Ignoring files

Create a file in `Demo/` called `.gitignore`. This file should contain one line:
```
# .gitignore
*.config
```

Stage and commit this file and push it to your remote
```
git add .gitignore
git commit -m "Created a gitignore"
git push myFork master
```

Now create a file in `Demo` called `myfork.config` and put anything you want in it. See what happens when you try to stage this file:
```
git add Demo/myfork.config
The following paths are ignored by one of your .gitignore files:
Demo/myfork.config
Use -f if you really want to add them.
```

Now git will ignore any files that end in `.config` within the Demo folder.

## Branching

Create a new branch and check it out
```
git branch my-branch
git checkout my-branch
```

Add a few more lines to `rit.md`, and create a commit
```
git add rit.md
git commit -m "Included information about the SSE"
```

Do this a couple of times to develop in interseting history. Notice how if you check the log, the log now includes your branch and highlights the commit where the histories diverge. 

You can use `checkout` to get back to the state from a few commits ago
```
git checkout HEAD~1
```
Notice how now you are in a detached HEAD state. This isn't very useful. So return to the tip of your branch.
```
git checkout my-branch
```

## Merging

Notice how one of the projects collaborators has work done on a separate branch in `origin`, `tims-branch`. Pull this branch down into your local repo with 
```
git pull origin tims-branch
```

Now if you list your branches you should see `tims-branch`
```
git branch
  master
* my-branch
  tims-branch
```

We want to incorporate the changes from `tims-branch` into our local master. To do this, run
```
git checkout master
git merge tims-branch
```

It looks like Tim also modified the school colors. Since both you and Tim made different changes, Git isn't sure how to merge them. This is called a merge conflict. To see which files have a conflict, get the status
```
git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

      both modified:   Demo/rit.md
      
no changes added to commit (use "git add" and/or "git commit -a")
```

This tells you that `Demo/rit.md` has merge conflicts. Open up `Demo/rit.md` to see what happened. 
You'll notice that there's a merge conflict where the git 
```
 16 <<<<<<< HEAD
 17 Colors | Orange and White
 18 =======
 19 Colors | White and Orange
 20 >>>>>>> tims-branch
```

Fix the merge conflict within the file by removing the unecessary info, save your changes, and continue with the merge:
```
git add rit.md
git merge --continue
```

Now if you check the log on master, you'll see a merge commit that contains the changes from `tims-branch` and the changes from `master`

## Some Cool commands

### Ammending commits

Change the commit message from the last commit
```
git commit --amend -m "Merged my work with Tim's"
git push myFork master
```

### Stashing

Switch back to your branch
```
git checkout my-branch
```

Make some changes to `rit.md` by adding more lines and save those change, but do not commit them. Instead, attempt to checkout `master`
```
git checkout master
error: Your local changes to the following files would be overwritten by checkout:
        rit.md
Please commit your changes or stash them before you switch branches.
Aborting
```

Since there are changes in both `master` and `my-branch`, checking out master will overwrite those changes. To get around this, use `stash`
```
git stash
Saved working directory and index state WIP on my-branch: 34b6ccb .
HEAD is now at 34b6ccb
git checkout master
Switched to branch 'master'
Everything up to date. 
```

Now if you want to get those changes back,
```
git checkout my-branch
git stash pop
```

### Diff

Diff can be used to show you the difference between the working directory and the previous commit
```
git diff
```

## Rebasing

So now that Tim's work is in master, and master has advanced past where your branch was based off of, you may want to incorporate those changes into your branch. Do this using `rebase`:

```
git rebase master
```

Now open up `rit.md` to ensure that the merged work is in fact there. 

After this, you'll want to update your remote repository to accept the new base

```
git push -f myFork my-branch
```

From this state, your branch can merge seamlesly into master.

But before we do that, lets squash all of the commits

## Rebase interactive

Use `git log` to find the commit your branch was based off of. Rebase interactive does not include the commit you rebase onto.

Once you find the commit, use 
```
git rebase 8a4ebc0 -i
```

This will open up your text editor and you'll be presented with a list of commits that will be rebased. It will look something like this:
```
pick eac1380 Created my awesome new feature
pick 8d8a9d5 Whoops, found a bug
pick 393dff4 and another
pick 0902440 oops, please ignore
pick a4b1f72 think I got it this time
pick 19453bc I was wrong
```

Use your text editor to rewrite the file so that it looks like this:
```
r eac1380 Created my awesome new feature
f 8d8a9d5 Whoops, found a bug
f 393dff4 and another
f 0902440 oops, please ignore
f a4b1f72 think I got it this time
f 19453bc I was wrong
```

Then save and close your text editor. Git will then open your text editor once again, prompting your for a new commit message for the first commit. Modify the commit message, save the file, and close your text editor again. 

After the rebase completes, check the git log to see how this impacted your git history. Notice how all of your changes from this branch have become one commit. 

## Cherry Pick

Now pull down Mary's branch from the `origin`.
```
git pull marys-branch
```

You'll notice how there are several commits in this branch. Each commit adds a new file. However, let's say you only want to pull in the changes that include `ur.md`. 

First, figure out which commit, you want to cherry pick
```
git log
commit 4cd8e1ce13976d064bdcdfb62bf57d2a38db138e
Author: Sean Levorse <smlevorse@msn.com>
Date:   Wed Feb 13 14:08:00 2019 -0500

    Nazareth College

commit bb7f6e2719f6e0d99b1086544c1fd7a2a0d93384
Author: Sean Levorse <smlevorse@msn.com>
Date:   Wed Feb 13 14:06:49 2019 -0500

    University of Rochester

commit c08660e18a1cadfd4bd88fa0c2aa7485eca9e4cb
Author: Sean Levorse <smlevorse@msn.com>
Date:   Wed Feb 13 14:05:55 2019 -0500

    St. John Fisher

```

Notice that you want commit `bb7f6e2719f6e0d99b1086544c1fd7a2a0d93384`, or `bb7f6e2`

Rather than merge and undo work, use Cherry-Pick
```
git cherry-pick bb7f6e2
```

This creates a new commit on your branch with the changes from the cherry picked commit. 

