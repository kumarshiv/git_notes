Git comes with a tool called gitconfig that lets you get and set configuration variables that control all aspects of how Git looks and operates
$ git config --system -l  [/etc/gitconfig file:Contains values for every user on the system and all their repositories]
$ git config --global -l  [C:/Users/Shiv/.gitconfig file:golobal for Specific to your user]
$ git config --local -l  [config file in the git directory (that is, .git/config), Specific to that single repository.]
Each level overrides values in the previous level, so values in .git/config trump those in /etc/gitconfig.
If you want to check your settings, you can use the git config --list command to list all the settings Git can find at that point
You may see keys more than once, because Git reads the same key from different files (/etc/gitconfig and /.gitconfig, for example). In this case, Git uses the lastvalue for each unique key it sees

If you have never used git before, you need to do some setup first.
$ git config --global user.name "Your Name"
$ git config --global user.email "your_email@whatever.com"

and also for Windows users:
$ git config --global core.autocrlf true
$ git config --global core.safecrlf true

=>To set notepad++ as your default editor
$ git config --global core.editor "'C:/Program Files/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
=>p4merge diff and merge tool
git config --global diff.tool p4merge
git config --global difftool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
->To run $ git difftool
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
->To run $ git mergetool

=>If you want to check your settings, you can use the git config -l command to list all the settings .
=>Remove value from the global settings
$ git config --global --unset-all user.name

=>You can get the manpage help for any command (let say config) by running
$ git help config

Initializing a Repository in an Existing Directory
If you’re starting to track an existing project in Git, you need to go to the project’s directory and type
$ git init
This creates a new subdirectory named .git that contains all of your necessary repository files — a Git repository skeleton. At this point, nothing in your project is tracked yet. (See Chapter 9 for more information about exactly what files are contained in the .git directory you just created.)
If you want to start version-controlling existing files (as opposed to an empty directory), you should probably begin tracking those files and do an initial commit. You can accomplish that with a few git add commands that specify the files you want to track, followed by a commit:
$ git add '*.c'
$ git add README
$ git commit -m 'initial project version'

=>git add command (it’s a multipurpose command — you use it to begin tracking new files, to stage files, and to do other things like marking merge-conflicted files as resolved)
//shortcut for adding in all the changes to the files in the current directory and below
git add .

=>$ git diff command result tells you the changes you’ve made that you haven’t yet staged.
[That command compares what is in your working directory with what is in your
staging area.]
=>git diff –-staged command compares your staged changes to your last commit
[To exit VIM editor use :q or :q! or :wq command]

=>git diff HEAD 
[show diff of all staged or unstaged changes, i.e. difference between your working directory and the last commit, ignoring the staging area.]


=>If you want to skip the staging area, Git provides a simple shortcut
$ git commit -a -m 'added new benchmarks'
Git automatically stage every file that is already tracked before doing the commit, letting you skip the git add or git rm part. But you still need to run 'git add' to start tracking new files.

=> If you type gitk on the command line in your project, you should see visual git log tool.
=> The git rm command remove a file from Git and also removes the file from your working directory.
=> Removing one file is great and all, but what if you want to remove an entire folder? You can use the recursive option on git rm:
git rm -r <folder_of_cats>
This will recursively remove all folders and files from the given directory.

=>$ git log
Use git log --summary to see more information for each commit.
One of the more helpful options is -p, which shows the diff introduced in each commit. You can also use -2, which limits the output to only the last two entries
$ git log -p -2

Other options:
git log --pretty=oneline --max-count=2
git log --pretty=oneline --since='5 minutes ago'
git log --pretty=oneline --until='5 minutes ago'
git log --pretty=oneline --author=<your name>
git log --pretty=oneline --all

I like the following log format for most of my work
git log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short

=>Common Aliases
Add the following to the .gitconfig file in your $HOME directory

[alias]
hist = log --pretty=format:'%h %ad | %s%d [%an]' --graph --date=short

With these aliases defined in the .gitconfig file you can type $ git hist which allow you to avoid the really long log command
=> GETTING OLD VERSIONS
$ git checkout <hash>
Note: Any changes that are committed in this state are only remembered as long as you don’t switch to a different branch. As soon as you checkout a new branch or tag, the detached commits will be “lost” (because HEAD has moved). If you want to save commits done in a detached state, you need to create a branch to remember the commits.
Return the latest version in the master branch
$ git checkout master

=>Tagging
$ git tag –a v1
Now you can refer to the current version of the program as v1.
[When you run the git tag -a command, Git will open your editor and have you write a tag message, just like you would write a commit message]

Tagging Previous Versions
$ git checkout v1^ [or provide hash of any other commit]

=>How to amend an existing commit.Let’s amend the previous commit to include the email change.
$ git add <file>
$ git commit --amend -m "Add an author/email comment"
you run this command immediately after your previous commit.
You can achieve the same effect by resetting the branch back one commit and then recommitting the new changes.

=> Revert changes (before stagging) in the working directory
$ git checkout <file>

=> Revert changes that have been staged (Reset the staging area)
$ git reset HEAD <file>
The reset command (by default) doesn’t change the working directory. We can use the checkout command to remove the unwanted change from the working directory.
$ git checkout <file>

=>REMOVING COMMITS FROM A BRANCH
git reset --soft
The first thing git reset does is undo the last commit and put the files back onto the stage. If you include the --soft flag this is where it stops. 
$ git reset --hard v1 or $ git reset --hard <hash>
The option --hard make your working directory look like the index, unstage files and undo any changes made since the last commit. This is the most dangerous option and not working directory safe.
$ git hist [ok]
$ git hist --all [bad commits are still in the repository, why?]
Commits that are unreferenced remain in the repository until the system runs the garbage collection software.
e.g. Undo a commit, making it a topic branch 
$ git branch topic/wip<1>
$ git reset --hard HEAD~3  <2>
$ git checkout topic/wip<3>
1.	You have made some commits, but realize they were premature to be in the "master" branch. You want to continue polishing them in a topic branch, so create "topic/wip" branch off of the current HEAD. 
2.	Rewind the master branch to get rid of those three commits. 
3.	Switch to "topic/wip" branch and keep working. 

=> Revert changes that have been committed to a local repository
$ git revert HEAD
This will pop you into the editor. You can edit the default commit message or leave it as is. The changes will automatically reverted.
We can revert any arbitrary commit earlier in history by simply specifying its hash value
[However, both the original commit and the “undoing” commit are visible in the branch history (using the git log command)]

How to amend an existing commit.Let’s amend the previous commit to include the email change.
$ git add <file>
$ git commit --amend -m "Add an author/email comment"

Remove files from the staging area
By default, a 'git rm file' will remove the file from the staging area entirely and also off your disk (the working directory). To leave the file in the working directory, you can use git rm --cached .

Move a file within a repository
$ git mv <file><dir>
$git commit -m "Moved hello.txt to lib"

git stash
[Stashing takes the current state of the working directory and index, puts it on a stack for later, and gives you back a clean working directory. It will then leave you at the state of the last commit.]
git stash list [view stashes currently on the stack]
git stash apply [grab the item from the stash list and apply to current working directory]
git stash drop [remove an item from the stash list]
 
Branching
Let we have a file called lib/hello.rb and then we created new branch ‘greet’
$ git checkout -b greet
then we have edited lib/hello.rb, we can noticed that files are different in two branches
 
Merging
We have created new file ‘README’ in Mater Branch. Let’s go back to the greet branch and merge master onto greet.
$ git checkout greet
$ git merge master

Before Merger ($ git hist --all)
 
After Merger ($ git hist --all)


Important: By merging master into your greet branch periodically, you can pick up any changes to master and keep your changes in greet compatible with changes in the mainline.

Git for Windows tip: Use P4Merge as mergetool
1)	Download P4Merge: Visual Merge Tool from http://www.perforce.com/perforce/downloads/component.html
2)	In the installer for P4Merge you can choose which components you wish to install, you only need the Visual Merge Tool (P4Merge).
3)	Install p4merge and then set it as your merge tool for git by running the following two config commands:
a.	gitconfig --global merge.tool p4merge
b.	gitconfig --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
4)	P4Merge can be used for doing diffs and merges. If using P4Merge for diffs then call:
git difftool
If the file you want to compare is already staged then use the –cached switch after that command.
5)	When Git tells you that there has been conflict, to resolve it type:
git mergetool
And this is what a 3-way merge looks like.
(1) remote version (the master branch) (2) base version (3) local version (local branch)
Below is the final file. Click on desired colored icon and save the changes.


Cloning Repositories
=>Go to the working directory and make a clone of your hello repository.
$ git clone hello cloned_hello

=>What is origin?
$ git remote show origin
Remote repositories typically live on a separate machine, possibly a centralized server. As we can see here, however, they can just as well point to a repository on the same machine. There is nothing particularly special about the name “origin”.

=>Fetching Changes from remote (hello) local (cloned_hello)
$ git fetch origin
the “git fetch” command will fetch new commits from the remote repository, but it will not merge these commits into the local branches.

=>$ git merge origin/master
Even though “git fetch” does not merge the changes, we can still manually merge the changes from the remote repository.

=>git pull is equivalent to a git fetch followed by a git merge.

Add a local branch(greet) that tracks a remote branch(greet)
The branches starting with ‘remotes’ or ‘origin’ are branches from the original repo.
$ git branch --track greet origin/greet
Branch ‘greet’ will be set up to track remote greet from origin (origin/greet)
Creating Bare Repositories for sharing [Server Side]
Bare repositories (without working directories [hello]) are usually used for sharing, usually shared on some sort of network server.
$ git clone --bare hello hello.git
The convention is that repositories ending in ‘.git’ like git://github.com/paulboone/ticgit.git are bare repositories.
Pulling Shared Changes[Client Side]
Let’s pull down the changes just pushed to the shared repo. ’shd’ is the short name of the repository (hello.git) receiving the changes we are pulling.
$ git remote add shd ../hello.git
$ git branch --track shd master
$ git pull shd master [Note: git pull is equivalent to a git fetch followed by a git merge]
How to push any change in working directory to the bare repository 
=>Made required changes in the working directory (hello).
=>Use bare repository with local name ‘shared’.
$ git remote add shared ../hello.git
=>Now push any change in working directory (hello) to the shared repo.
$ git push shared master
‘shared’ is the local name of the bare repository receiving the changes we are pushing. We had to explicitly name the branch ‘master’ that was receiving the push.
Hosting your Git Repositories
(From the work directory [i.e ../hello])
$ git daemon --verbose --export-all --base-path=.
Now, in a separate terminal window, go to your work directory
$ git clone git://localhost/hello.git network_hello
You should see a copy of hello project.
 

Working with Remotes

If you’ve cloned your repository, you should at least see origin — that is the default name Git gives to the
server you cloned from:

$ git clone git://github.com/schacon/ticgit.git

When you clone a repository, it generally automatically creates a master branch that tracks origin/master. That’s why git push and git pull work out of the box with no other arguments.

=>To see which remote servers you have configured, you can run the git remote -v command.

=>To add a new remote Git repository as a shortname you can reference easily, run git remote add [shortname] [url]
e.g. $ git remote add pb git://github.com/paulboone/ticgit.git

You can run git fetch pbto fetch all the information that Paul has

=>It’s important to note that the fetch command pulls the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.

$ git merge pb/master [or simply $ git merge origin/master]

=> If you have a branch set up to track a remote branch, you can use the git pull command to automatically fetch and then merge a remote branch into your current branch; and by default, the git clone command automatically sets up your local master branch to track the remote master branch on the server you cloned from (assuming the remote has a master branch).

$ git pull pb master [or simply git pull from master branch]
[Sometimes when you go to pull you may have changes you don't want to commit just yet. One option you have, other than commiting, is to stash the changes.]


Pushing

$ git push (remote) (branch) 

e.g. $ git push -u origin serverfix
(The -u tells Git to remember the parameters, so that next time we can simply run git push)

Which means, “Take my serverfix local branch and push it to update the remote’s serverfix branch.”

You can use this format to push a local branch into a remote branch that is named differently. If you didn’t want it to be called serverfix on the remote, you could instead run git push origin serverfix:awesomebranch to push your local serverfix branch to the awesomebranch branch on the remote project.

=>It’s important to note that when you do a fetch that brings down new remote branches, you don’t automatically have local, editable copies of them. In other words, in this case, you don’t have a new serverfix branch—you only have an origin/serverfix pointer that you can’t modify. 
To merge this work into your current working branch, you can run git merge origin/serverfix. If you want your own serverfix branch that you can work on, you can base it off your remote branch:

$ git checkout -b serverfix origin/serverfix

This gives you a local branch that you can work on that starts where origin/serverfix is.


Tracking Branches

Checking out a local branch from a remote branch automatically creates what is called a tracking branch.

If you’re on a tracking branch and type git push, Git automatically knows which server and branch to push to. Also, running git pull while on one of these branches fetches all the remote references and then automatically merges in the corresponding remote branch.

When you clone a repository, it generally automatically creates a master branch that tracks origin/master. That’s why git push and git pull work out of the box with no other arguments. However, you can set up other tracking branches if you wish—ones that don’t track branches on origin and don’t track the master branch. The simple case is the example you just saw, running git checkout -b [branch] [remotename]/[branch]. If you have Git version 1.6.2 or later, you can also use the --track shorthand:

$ git checkout --track origin/serverfix

To set up a local branch with a different name than the remote branch, you can easily use the first version with a different local branch name:

$ git checkout -b sf origin/serverfix

Now, your local branch sf will automatically push to and pull from origin/serverfix.

Deleting Remote Branches

You can delete a remote branch using the rather obtuse syntax git push [remotename] :[branch]. If you want to delete your serverfix branch from the server, you run the following:

$ git push origin :serverfix
 
Generally you will do a number of commits locally, then fetch data from the online shared repository you cloned the project from to get up to date, merge any new work into the stuff you did, then push your changes back up.
EXAMPLES
•	Update the remote-tracking branches for the repository you cloned from, then merge one of them into your current branch: 
$ git pull, git pull origin
Normally the branch merged in is the HEAD of the remote repository, but the choice is determined by the branch.<name>.remote and branch.<name>.merge options; see git-config(1) for details.
•	Merge into the current branch the remote branch next: 
$ git pull origin next
This leaves a copy of next temporarily in FETCH_HEAD, but does not update any remote-tracking branches. Using remote-tracking branches, the same can be done by invoking fetch and merge:
$ git fetch origin
$ git merge origin/next
If you tried a pull which resulted in complex conflicts and would want to start over, you can recover with git reset.
 
Upgrade Moodle to new major version using git
git checkout MOODLE_21_STABLE
git pull # Ensure the 2.1 branch is up to date against upstream
git checkout local_21_STABLE
git merge MOODLE_21_STABLE # Ensure your local branch is up to date
php admin/cli/upgrade.php # It seems sensible to upgrade to the latest 2.1 at this point
git checkout MOODLE_22_STABLE
git pull # As above, but with the 2.2 branch
git checkout -b local_22_STABLE MOODLE_22_STABLE
mkdir .patches
git format-patch -o .patches MOODLE_21_STABLE..local_21_STABLE # Create the patches - check you've got what you were expecting
git am -3 .patches/* # Apply the patches
rm -rf .patches
php admin/cli/upgrade.php # Run the upgrade
If you encounter and fix conflicts while running git am, ensure you run git add {filename} before running git am --resolved or it will complain about "unmerged paths in your index".
When a patch does not apply cleanly, the command tries fall back on 3-way merge (see the -3 parameter). If conflicts occur during the procedure, you can either deal with them and then use `git am --continue` or abort the whole procedure with `git am --abort`.

