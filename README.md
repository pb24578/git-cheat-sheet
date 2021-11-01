# Git Cheat Sheet
This is a personal git cheat sheet that I use to learn and understand how to use rudementary git commands. It follows a sequential flow from starting a project, staging files, committing changes, then pushing those commits to the repository.

Useful Links:
1. Install the Git CLI (command line interface): https://git-scm.com/  
2. If you mess up anything when using git, you can follow this guide: https://ohshitgit.com/

# Initiating Repository
### 1. Init
There is no need to use commands such as ```git init``` to initiate a repository. Simply go to GitHub and initiate the repository there.

### 2. Clone
If we wish to clone an existing repository, then execute either one of these commands:
- To clone using the same name as the repository: ```git clone <repo URL>```
- To clone using a custom name: ```git clone <repo URL> <name>```

Now, the repository should be cloned as a folder in the directory that you executed the command from.

If you want to clone a repository from another account to your personal account, you must first "Fork" that repository on GitHub. Now git clone the forked repository URL and start developing from there.

### 3. Remote (Optional, only follow if you forked a repository)
If you forked a repository from another account, then you need to configure a remote for the fork. A remote is another repository that you may or may not make want to make changes to.

If you type ```git remote -v``` you will see this below:
```bash
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

As you notice, you only see the repository for your fork but not the remote. We want to also include the upstream (original) repository, too. Execute ```git remote add upstream <original repo's .git URL>``` to include the original repository as a remote.

Now if you type ```git remote -v```, you will see this below:
```bash
> origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
> origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

Now there are two remotes, which are origin and upstream. We can now use version control on either one of them!

### 4. Branch
Now that our repository has been cloned (and maybe added a remote if it was forked), we can start creating our own development version called a "Branch".

- To create a new branch and switch into it, execute ```git checkout -b <branch name>```
- To check if the branch was created, execute ```git branch -a```
- To only switch to another branch, execute ```git checkout <branch name>```. This will actually update your working code to the most recent commit of that branch, so your code base will change to the commits within that branch
- To rename the branch, execute ```git branch -m <new branch name>``` then push it using ```git push <remote name> <new branch name>```
- To delete the branch locally execute ```git branch -D <branch name>```, then to delete the branch remotely execute ```git push -d <remote name> <branch name>```  

If we forked this repository, we also need to connect the remote for this new branch by executing ```git remote add <remote name> <branch name>```. For example, for upstream, we execute ```git remote add upstream <branch name>```.

# Making Changes
### 1. Stash (Optional, only if you want to stash away changes for later)
Stashing allows you to store your dirty or staged changes in a stack of unfinished changes that can be applied later.
- Good to use for very messy code, especially during merge conflicts

You can view your changes using ```git diff```. Type 'q' to quit the git diff console.  

To create a new stash, execute ```git stash save <stash name>```.  
To view the list of stashes, execute```git stash list```.  

To apply a specific stash, execute ```git stash apply stash@{<stash id>}```. This will add back your changes to the specific files that were edited. Note that if you edit a file that has been stashed, then there will exist merge conflicts within the stash itself.

To delete a stash and the changes inside of the stash (will permanently delete the changes in the stash), execute ```git stash drop stash@{<stash id>}```.

### 2. Stage
If you make changes to your repository and want to track that you've created these changes, then you must stage these changes. Staging is also very helpful because it allows you to separate the changes made between commits, so it organizes your changes even more.

In order to stage, execute either:
- To stage only a single file: ```git add <file>```
- To stage a folder: ```git add <folder>```
- To stage all files recursively in the current directory: ```git add .```
- To stage all files of the repository: ```git add -A```

After you stage files, you can execute ```git status``` which would display the changes in your current branch. It will show everything from what files were modified, deleted, not yet staged, not yet committed, etc.

To revert a git add, execute ```git reset <file>``` or to revert all changes execute ```git reset```.

### 3. Commit
If you want to record that you made changes, then you commit. Only commit after you've staged changes, or else it won't be able to see the record the tracked changes!

To commit, execute: ```git commit -m <message> -m <description>```.  
To undo this commit, execute ```git revert <SHA1 commit hash>```.  
- To find the commit's hash, execute ```git log``` and search for it in the log

To completely rollback to a specified commit, execute ```git reset --hard <SHA1 commit hash>```. This will change your working code base into the state of the previous commit's code base.

### 4. Push
Once we finished making our commits for a new update, then we can push our changes to a repository. Only push after you have created commits, or else there won't be anything to push.

To push, execute: ```git push <remote name> <branch name>```.
- This means that we push our changes in <branch name> into <remote name>
- The changes will show up in the remote repository in <branch name>

A common example of git version control flow would look like this:
```bash
git add file1.c
git add file2.c
git commit -m "Initialize a few C programs" -m "These programs are necessary for the application"
git add file3.c
git commit -m "Create another C program" -m "This file is also necessary"
git push origin develop
```

Now on GitHub, you would see two commits titled "Initialize a few C programs" and "Create another C program".
  
# Rebasing Changes
Sometimes we may make a commit and write the wrong the description, write the wrong title, make too many individual commits, or mess up the sequence of commits. We can resolve these issues using the ```git rebase -i HEAD~<n>``` or ```git rebase -i <SHA1 commit hash>``` command.
  
When you execute the ```git rebase -i <ARGS>``` command, you should see multiple options to modify your commits: pick, reword, edit, squash, fixup, exec, drop.
  - p, pick = use commit
  - r, reword = use commit, but edit the commit message
  - e, edit = use commit, but stop for amending
  - s, squash = use commit, but meld into previous commit
  - f, fixup = like "squash", but discard this commit's log message
  - x, exec = run command (the rest of the line) using shell
  - d, drop = remove commit
  
### 1. Rewording commit message
Execute ```git rebase -i <commit-sha1>``` using the SHA1 of the commit you want to edit. Then change the "pick" part before the commit message to "reword" and save the file. Once the file is saved, a new file will open in your Terminal to edit the commit message. Once you edit the commit message, save.
  
The commit message should now be changed.
  
### 2. Squashing commits
If commits are together and should probably be combined into a single commit (to make the git tree to look easier to read), then you can use the "squash" option. Execute ```git rebase -i HEAD~<n>``` where n is the n last number of commits. The menu should appear again. Pick a single commit and any commits below the pick commit that you want to squash into the pick commit.
  
Here's an example:
```
pick 953be09 Code the header
pick 99fc6a8 Code the body
pick 8ddb4bc Code the footer
pick a784c37 Progress on unit tests
squash 4ec3faf Progress more unit tests again
squash b8ffdf6 Progress even more unit tests again
```
  
This means the code changes from the commit a784c37, 4ec3faf, and b8ffdf6 will all be squashed into the "Progress on unit tests" commit.
  
### 3. Resetting git rebase
If you want to abort from a git rebase before the changes are saved, then you can simply do ```git rebase --abort```. However, if you git rebase and want to revert the rebase that you already saved, you can execute ```git reflog``` and ```git reset --hard``` reset back into the HEAD@{n} reference.
  
For example, execute ```git reflog``` and HEAD@{3} was the commit reference before you rebased, then you can execute ```git reset --hard HEAD@{3}``` to revert your rebase changes.
  
### 4. Solving messy git trees from rebase using git cherry-pick
What if we already pushed the un-squashed commits into the branch and then push a single squashed commit? This would create a really messy git tree when we pushed the squashed commits into the branch. The goal is to make a clean and linear git tree that just only has the squashed commit.
  
In order to resolve this, checkout on another branch from your master branch. We can then cherry pick the squashed commit in our new branch from the old branch. To perform this, we can do ```git cherry-pick <commit-hash>``` where the commit-hash is the squashed commit.
  
Now, you can push the new branch and the git tree should be much cleaner in the new branch.

# Receiving Changes
### 1. Fetch
Fetching downloads any changes from the repository. To download all changes (such as new branches), simply execute ```git fetch```. This will download changes but not modify anything in your working code. Fetching only downloads the changes, but it does not change any files or create any commits.

To fetch a specific branch, execute ```git fetch <remote> <branch name>```. This will now create a local copy named ```<remote>/<branch name>```, which we can merge later.

### 2. Merge
If we want to merge changes from another branch, then we can merge them and change the files in our current branch. First, make sure to git fetch or else those changes will not be downloaded.

To merge, execute ```git merge <remote>/<branch name>```.  
To revert the previous commit (which would be the merge commit), execute ```git revert HEAD```.

Note that there may exist a merge conflict, which occurs whenever two files from two branches modify the same region of a file while merging. Git will add merge conflict markers on the files that had the merge conflicts, then you must go in and resolve them yourself following the markers. These markers separate the conflicts as either the "incoming change" or the "current change". Once you change the files, you can stage, commit, then push the resolved fixes to the remote repository.
- A good text editor (such as VS Code) or IDE will make it easier for you to view these merge markers

### 3. Pull
Pulling executes a fetch then a merge.

To pull, execute ```git pull <remote> <branch name>```.

# Pull Requests (PR)
If you been committing changes to your repository and have resolved merge conflicts from the branch you want to make a pull request into, then you can create a pull request on GitHub. Your pull request will be reviewed by another engineer (typically a senior) and should give you a code review before merging your pull request into that engineer's branch.

If you make more commits in your branch, then the pull request will automatically show those commits as well. Therefore, there is no need to close your current pull request and create another one. A pull request is like a discussion, particularly for code reviews before merging into another engineer's branch.

# TOWER's Git Cheat Sheet
<img src="git-cheat-sheet.png" width=65% height=65% />
