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
