# GitTutorial

This tutorial is meant as a brief introduction to git, with focus on the tools needed to contribute to the HEP-FCC repositories. This repository [FCC-HEP/GitTutorial](https://github.com/HEP-FCC/GitTutorial) is meant as a playground for familiarizing yourself with the version control system (VCS) chosen for HEP-FCC: `git`.

## About git

git is a *distributed* version control system (dVCS), in strong contrast with SVN or CVS which are *centralized*. The main distinction between the the two is the fact that in a dVCS each user gets his/her own copy of the repository, while in the case of a centralized system, all version control operations require interacting with the central system hosting the code repository.

This has important implications, and means that the git workflow is *significantly* different from that of CVS or SVN. In particular, *committing* changes merely affects your *own*, local, copy of the repository. Additional steps are required to share your work with others. While this may seem like adding un-needed extra steps, this allows you, e.g. , to benefit from version control while developing features without the need to push your work to others until it's functional/tested (like in the case of SVN), or to be able to test alternative development paths before picking the best one. Altough a minor benefit in today's connected world, you can also work offline while still maintaining a history of your work. In addition, you also carry the complete history of the project with you! (Don't worry about the extra size: git is very efficient at compressing things.)

Besices the choice of git as our VCS, we also chose to use GitHub to host the HEP-FCC repositories. GitHub is a collaborative platform for working on software projects using git. It offers many features such as issue tracking and possibilities to comment code.
