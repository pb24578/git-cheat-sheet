# Git Cheat Sheet
This is a personal git cheat sheet that I use to learn and understand how to use rudementary git commands. It follows a sequential flow from starting a project, staging files, committing changes, then pushing those commits to the repository.

You must install he Git CLI (command line interface) from https://git-scm.com/.

# Initiating Repository
### 1. Init
There is no need to use commands such as ```git init``` to initiate a repository. Simply go to GitHub and initiate the repository there.

### 2. Cloning
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

### 4. Branching
Now that our repository has been cloned (and maybe added a remote if it was forked), we can start creating our own development version called a "Branch".

To create a new branch and switch into it, execute ```git checkout -b <branch name>```. Execute ```git branch -a``` to check if the branch was created successfully.

If we forked this repository, we also need to connect the remote for this new branch by executing ```git remote add <remote name> <branch name>```. For example, for upstream, we do ```git remote add upstream <branch name>```.

# Making Changes
### 1. Staging
If you make changes to your repository and want to track that you've created these changes, then you must stage these changes. Staging is also very helpful because it allows you to separate the changes made between commits, so it organizes your changes even more.

In order to stage, execute either:
- To stage only a single file: ```git add <file>```
- To stage a folder: ```git add <folder>```
- To stage all files recursively in the current directory: ```git add .```
- To stage all files of the repository: ```git add -A```

After you stage files, you can execute ```git status``` which would display the changes in your current branch. It will show everything from what files were modified, deleted, not yet staged, not yet committed, etc.

### 2. Committing
If you want to record that you made changes, then you commit. Only commit after you've staged changes, or else it won't be able to see the record the tracked changes!

To commit, execute: ```git commit -m <message> -m <description>```.

### 3. Pushing
Once we finished making our commits for a new update, then we can push our changes to a repository. Only push after you have created commits, or else there won't be anything to push.

To push, execute: ```git push <remote name> <branch name>```.

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
