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
