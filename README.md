# Git Cheat Sheet
This is a personal git cheat sheet that I use to learn and understand how to use rudementary git commands. It follows a sequential flow from starting a project, stagging files, committing changes, then pushing those commits to the repository.

First you must install he Git CLI (command line interface) from https://git-scm.com/.

# Initiating Repository
### 1. Init
There is no need to use commands such as ```git init``` to initiate a reppsitory. Simply go to GitHub and initiate the repository there. Then, call ```git clone``` on that repository as described below.

### 2. Cloning
If we wish to clone an existing repository, then execute either one of these commands:
- To clone using the same name as the repository: ```git clone <repository URL>```
- To clone using a custom name: ```git clone <repository URL> <name>```

Now, the repository should be cloned as a folder in the directory that you executed the command from.

If you want to clone a repository from another account to your personal account, you must first "Fork" that repository on GitHub. Now git clone the forked repository URL and start developing from there.

### 3. Remote
If you forked a repository from another account, then you need to configure a remote for the fork. A remote is the common  (also referred to as the parent or original) repository of all the related forked repositories.

If you type ```git remote -v``` you will see this below:
```bash
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
> origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

As you notice, you only see the repository for your fork but not the remote. We want to also include the remote repository, too. Execute ```git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git``` to include the remote.

Now if you type ```git remote -v```, you will see this below:
```bash
> origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
> origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

Now there are two remotes, which are origin and upstream. We can now use version control on either one of them!
