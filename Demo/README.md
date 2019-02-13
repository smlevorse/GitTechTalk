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

## Fetch and Pull

There may be some chages in `origin` that aren't in your branch. For example, it looks like your collaborator, Tim, has added some work in his branch, `tims-branch`.



