# GitTutorial

This tutorial is meant as a brief introduction to git, with focus on the tools needed to contribute to the HEP-FCC repositories. This repository [FCC-HEP/GitTutorial](https://github.com/HEP-FCC/GitTutorial) is meant as a playground for familiarizing yourself with the version control system (VCS) chosen for HEP-FCC: `git`.

## About git

git is a *distributed* version control system (dVCS), in strong contrast with SVN or CVS which are *centralized*. The main distinction between the the two is the fact that in a dVCS each user gets his/her own copy of the repository, while in the case of a centralized system, all version control operations require interacting with the central system hosting the code repository.

This has important implications, and means that the git workflow is *significantly* different from that of CVS or SVN. In particular, *committing* changes merely affects your *own*, local, copy of the repository. Additional steps are required to share your work with others. While this may seem like adding un-needed extra steps, this allows you, e.g. , to benefit from version control while developing features without the need to push your work to others until it's functional/tested (like in the case of SVN), or to be able to test alternative development paths before picking the best one. Altough a minor benefit in today's connected world, you can also work offline while still maintaining a history of your work. In addition, you also carry the complete history of the project with you! (Don't worry about the extra size: git is very efficient at compressing things.)

Besices the choice of git as our VCS, we also chose to use GitHub to host the HEP-FCC repositories. GitHub is a collaborative platform for working on software projects using git. It offers many features such as issue tracking and possibilities to comment code.


But enough chatting, let's start with a few examples.

## Forking a GitHub project

Before you start, you need to [create an account with GitHub](https://github.com/join). Once this step is complete, and you've logged in, come back to this page ([https://github.com/HEP-FCC/GitTutorial](https://github.com/HEP-FCC/GitTutorial)) to continue this tutorial.

In order to contribute to the HEP-FCC code, you need first to take a copy of the main code repository so that you can improve it with your contribution. This process is referred as *forking*. Forking a repository allows you to freely experiment with changes without affecting the original project [[1](https://help.github.com/articles/fork-a-repo/)]. When your contirbution is tested and worth sharing with *users*, you should submit a *pull request* (more on this later).

Go ahead and click the "Fork" button at the top right page of this page. If asked, select your user identity and you're good to go. You should now be redirected to an address of the form https://github.com/**your GitHub user**/GitTutorial. We are now ready to get started.

## Cloning your fork to your local machine

The main [FCC-HEP/GitTutorial](https://github.com/HEP-FCC/GitTutorial) repository as well as your own fork are *bare* repositories, which means they serve only the purpose of hosting code. In order to add files ans modify them, you need a local copy, or a checkout in VCS lingo. In git, this process is done thorough *cloning*. Cloning will do several things:

1. It will create a local copy of the repository you're cloning.
2. It will assign the original repository you cloned from the name **origin** (*remote* in git lingo).
3. It will checkout the code, i.e., create a local *working copy* which you can edit and whose files can be put under version control. The default behaviour is to checkout the **master** branch.

```
git clone https://github.com/[YOUR_GITHUB_USER]/GitTutorial.git
```

Alternatively, if you have setup [SSH authentication on GitHub](https://help.github.com/articles/generating-ssh-keys/), you can use

```
git clone git@github.com:[YOUR_GITHUB_USER]/GitTutorial.git
```

Throughout this tutorial, the `git@github.com` form will be used instead of the `https://gitub.com`.

## Understanding how git does version control

Git stores your content and refers to it using a *hash*, which is a 160-bit number commonly represented using 40 hexadecimal characters. For example, the git hash of the initial commit of this project is 3bc8fd1ac3041f3a8aac05eec76ec3bdc347332c. This hash uniquely determines a snapshot your project's file structure. This is more or less the equivalent of the revision number in SVN. However, hashes are not the most common way to remember a project's state. Instead, like in SVN, we will use branches to refer to a given sate of our project. In git, a branch is just a pointer to a specific commit hash. This is different from SVN where a branch is in effect a subfolder with a copy of your files (that is partially optimized for efficient storage). In git, you cannot checkout two versions of your project (i.e. two different branches) in the same working copy.

You are free to choose the name you want for your branch, but one name is commonly *reserved* for a special purpose: **master**. The master branch is the one that is checked out by default when you clone a repistory (unless you explicitly specify otherwise) and usually points to the latest *production* version of your project. It is the one a user of your package would checkout to build from. This is particulary true of the HEP-HCC master, which is the one production jobs should use (it should be the same as the latest tag. Development is not done on this branch, unless a feature is validated. In some sense (but not really either), it corresponds to the SVN `trunk` with the additional guarantee that the code is tested and working (but it can still have bugs).

Some projects out there use a `develop` branch to refer to a SVN trunk. The develop branch is meant to be shared with developers and represents the latest development version of your project. A common workflow is for the master branch to a hash that the development branch was pointing to at some point in time. However, the GitHub workflow doesn't use the develop branch, so we will not use it.

Contraty to SVN, git branches are very cheap to make and are just **a pointer to a git hash**, i.e. a convenient alias, nothing more. They are however very useful at helping you track various development ideas.


# Basic git operations

## A first encounter with your working copy

In this section, we'll use some basic git features to get an idea on how things work. Let's develop a new feature!

First, we'll make sure to start of the master branch. Remember, the original clone has already checkout the master branch, as can be seen by running `git branch` from your working copy (the `GitTutorial` folder created by the clone operation above.

```
GitTutorial $ git branch
* master
```

The asterisk at the begining of the line indicates the branch that is currently checked out.

## Creating a feature branch and adding content

Now that we've confirmed we are in the master branch, we are ready to start working on our first contribution. So let's create a *feature branch*. A feature branch is a convenient naming scheme for branches (basically prefixing them with `feature/` that will host the development of a specific software feature. In this example, we'll create a file containing some particles with their mass. Let's create a new branch:

```
GitTutorial $ git checkout -b feature/particleMasses
Switched to a new branch 'feature/particleMasses'
```

The branch name is up to you, but better use something meaningful. Note that you can also use other prefixes such as `bugfix/` in case of bug fixes. This branch will be used for as long as our feature requires it. Note: `git checkout -b branch_name` is a shortcut for `git branch branch_name && git checkout branch_name`.

Now git branch shows two local branches which point to the same git hash (as shown using the `--verbose` argument):

```
GitTutorial $ git branch --verbose
* feature/particleMasses  3bc8fd1 Initial commit
  master                  3bc8fd1 Initial commit
```

You'll notice that that the hash is not 40 hex digits long. That's because git will only show you a part of the hash, enough to guarantee that there's no ambiguity. Obviously, the hash you see will differ from what is in this tutorial as the project evolves.

We are now ready to add some content to our repository.

```
GitTutorial $ cat Particles/Masses.txt
# Name	Mass (in MeV/c2)
Electron	0.511
GitTutorial $ git add Particles/Masses.txt
```

After we've edited our file, we need to tell git that we want to have it under version control. For this, we use the `add` command.

At any time, one can query the status of a git repository using `git status`:

```
GitTutorial $ git status
On branch feature/particleMasses
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   Particles/Masses.txt

Untracked files not listed (use -u option to show untracked files)
```

We have just added our file to be staged as part of the next commit. But git also shows us which commands to use to unstage the file (so that it doesn't get committed with the next commit operation) or to list files that are not being tracked by git. In general, git provides a lot of useful information on the command line.

`git add` is an important command that serves multiple purposes. Besides adding new files to a git repository, it allows to update the repository with the current version of a file already under version control (by adding it to be staged). In general, add here means "add (the local modifications to) my file the stage area so that I can later commit it".

## Commiting changes

Before commmitting, we need to let git know how we are so that commits can be correctly attributed. Once a commit is made, its author cannot be changed without modifying the hash corresponding to the commit (the commit metadata is included in the computation of the hash). For this and other git configuration items, one uses `git config`:

```
git config --global user.name "Your Name"
git config --global user.email "Your.Name@cern.ch"
```

The `--global` flag can be omitted, but the information will only be stored in the configuration file for the current working copy instead of a file in you home directory (`~/.gitconfig` on unix systems). It is recommened to use the global configuration file.

We can now commit our file using the `git commit` command:

```
GitTutorial $ git commit -m "Initializing the Masses.txt, adding the electron"
[feature/particleMasses 1c28ed3] Initializing the Masses.txt, adding the electron
 1 file changed, 2 insertions(+)
 create mode 100644 Particles/Masses.txt
```

Git shows us the branch we have committed to (feature/particleMasses), the new hash to which the branch now points to, as well as our commit message, specified with the `-m` flag. Note that if the flag is not specified, git will open whichever  In addition, it shows us how many files were affected and how many lines were added or removed. Running `git status` confirms that our feature branch now points to a different hash.

```
GitTutorial $ git branch --verbose
* feature/particleMasses 1c28ed3 Initializing the Masses.txt, adding the electron
  master                 3bc8fd1 Initial commit
```

Git status now tells us that there are no files known to git that are different in our working copy with respect to what is stored in **our local repository** (more on this later).

```
GitTutorial $ git status
On branch feature/particleMasses
nothing to commit (use -u to show untracked files)
```

One can also see the history of commits using `git log`:

```
GitTutorial $ git log
commit 1c28ed31f0cb28b79c96f7b601a32b09768424b9
Author: Karolos Potamianos <karolos.potamianos@cern.ch>
Date:   Thu Jan 29 01:19:29 2015 +0100

    Initializing the Masses.txt, adding the electron

commit 3bc8fd10e9061c7f0074c9354c65440e4d976937
Author: Karolos Potamianos <karolos.potamianos@cern.ch>
Date:   Wed Jan 28 19:27:18 2015 +0100

    Initial commit
```

Commits should be done as often as possible. Indeed, by committing often and into smaller chuncks belonging logically together, subsequent git operations (including conflict resolution) can be made much easier.

# Interacting with remote repositories

## Pushing your changes to your fork of GitTutorial

Now that we've added changes to our local repository, we would like to share it with the world by pushing it to our fork of GitTutorial on GitHub. This is done via the **push** command:

```
GitTutorial $ git push -u origin feature/particleMasses
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (7/7), 612 bytes | 0 bytes/s, done.
Total 7 (delta 0), reused 2 (delta 0)
To git@github.com:karolos-potamianos/GitTutorial.git
 * [new branch]      feature/particleMasses -> feature/particleMasses
Branch feature/particleMasses set up to track remote branch feature/particleMasses from origin.
```

Git will do some compressing in order to efficiently store your data, and will push it to the remote named origin (your fork). The `-u` flag is needed to tell git that you want to "link" your local branch feature/particleMasses with that from the origin repository. This is only required for the first push. For subsequent pushes after new local commits: simply use git push.

Like in the case of commits, you should push as often as possible to ensure proper backups.

## Making your modifications mainstream: pull requests

Now that your contribution is made public via your fork, you can tell the HEP-FCC maintainers that you have a feature ready to be brough in. To do so, GitHub as a feature called **pull request**. A pull request is an invitation to the maintainer of a project to bring in (merge in git lingo) your changes into the mainstream repository.

On your fork's webpage (https://github.com/**your GitHub user**/GitTutorial), you'll see a button to do a pull request (additional information available [here](https://help.github.com/articles/using-pull-requests/)). You can then review your changes and submit a description of your feature. Once you submit the request, a page like [this](https://github.com/HEP-FCC/GitTutorial/pull/1) is created to keep track of the pull request.

The goal of pull request is to allow for a review of the proposed code before it's added to the mainstream code base. Maintainers can request additional testing, endorsement, or stylistic corrections, etc. This ensures a form of uniformity of the code base. But because this process is a bit more tedious, it should only be used for complete features or bugfixes.

Once the reuest has been approved, the code is merged into the mainstream repository HEP-FCC/GitTutorial.

In fact a developer never pushes to the official repository, but instread invites the maintainers to pull from his/her repository.

## Pulling changes from a remote repository

So far, we've shown how to push changes to a remote repository but didn't address the important aspect of obtaining changes made by others. In the git lingo, this is called **pull**ing. This step should also happen as often as possible in order to be kept updated with the changes made by others.

For this purpose, we'll add a remote called **official**, which will link to the mainstream [HEP-FCC/GitTutorial](https://github.com/HEP-FCC/GitTutorial) repository:

```
GitTutorial $ git remote add official git@github.com:HEP-FCC/GitTutorial.git
GitTutorial $ git remote -v
official	git@github.com:HEP-FCC/GitTutorial.git (fetch)
official	git@github.com:HEP-FCC/GitTutorial.git (push)
origin	git@github.com:karolos-potamianos/GitTutorial.git (fetch)
origin	git@github.com:karolos-potamianos/GitTutorial.git (push)
```

This will be used to interact with the main repository.

We can now pull from the official repository. Because we have linked the master to our own fork, we need to explicity tell `git` what our intentions are:

```
GitTutorialLocal $ git fetch official master
remote: Counting objects: 1, done.
remote: Total 1 (delta 0), reused 1 (delta 0)
Unpacking objects: 100% (1/1), done.
From github.com:HEP-FCC/GitTutorial
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> official/master
```

It is intersting at this point to understand the state of our working copy.

```
GitTutorial $ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)
nothing to commit (use -u to show untracked files)
GitTutorial $ git branch --verbose --all
  feature/particleMasses                1c28ed3 Initializing the Masses.txt, adding the electron
* master                                afb9b55 [ahead 2] Merge pull request #1 from karolos-potamianos/feature/particleMasses
  remotes/official/master               afb9b55 Merge pull request #1 from karolos-potamianos/feature/particleMasses
  remotes/origin/HEAD                   -> origin/master
  remotes/origin/feature/particleMasses 1c28ed3 Initializing the Masses.txt, adding the electron
  remotes/origin/master                 3bc8fd1 Initial commit
```

We see that our branch is ahead of origin/master by two commits. The reason for this is that our fork's master hasn't been updated since the pull request was accepted. Therefore, that repository is not up to date. Let's go ahead ad update it:

```
GitTutorial $ git push
Counting objects: 1, done.
Writing objects: 100% (1/1), 287 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:karolos-potamianos/GitTutorial.git
   3bc8fd1..afb9b55  master -> master
GitTutorial $ git branch --verbose --all
  feature/particleMasses                1c28ed3 Initializing the Masses.txt, adding the electron
* master                                afb9b55 Merge pull request #1 from karolos-potamianos/feature/particleMasses
  remotes/official/master               afb9b55 Merge pull request #1 from karolos-potamianos/feature/particleMasses
  remotes/origin/HEAD                   -> origin/master
  remotes/origin/feature/particleMasses 1c28ed3 Initializing the Masses.txt, adding the electron
  remotes/origin/master                 afb9b55 Merge pull request #1 from karolos-potamianos/feature/particleMasses
```

We now notice that the [ahead 2] notification has disappeared.
