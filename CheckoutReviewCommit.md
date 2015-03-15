# Introduction #

PlayN uses Git, a distributed version control system. What this means is that, even though this page hosts a central repository, there can be many clone repositories with changes of their own, and then some of those can be merged back into the main repository.

This document describes the workflow for checking out code, making clones, reviewing patches, and committing code.

## Checking out PlayN (Linux) ##
For non-committers, checking out code is simple.
  1. Install Git - Follow the [installing Git](http://book.git-scm.com/2_installing_git.html) instructions. Ubuntu users can simply type:
```
apt-get install git-core
```
  1. Configure Git to [convert line endings on commit](http://help.github.com/line-endings/)
```
git config --global core.autocrlf input
```
  1. Checkout the code:
```
git clone https://github.com/threerings/playn.git
```
  1. Start PlayiNg :)

## Building with Eclipse and Maven (m2e) ##
Note: As of Eclipse Indigo, [Eclipse IDE for Java Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/indigor) includes m2eclipse by default. However,
[Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/indigor) does not. To install _Maven Integration for Eclipse_:
  * Select **Help → Install New Software**, select **All Available Sites** and search for **maven**.

To import PlayN into Eclipse, do the following:

  1. Select **File → Import** from the menu. Select **Maven → Existing Maven Projects** and click **Next**.
  1. Enter the directory into which you checked out PlayN in the **Root Directory** box and press enter.
  1. The various PlayN projects and sample projects will appear in the **Projects** listing. Then click **Finish** to import the projects.

## Contributing code ##
Contributing code to PlayN follows the standard [fork and pull](https://help.github.com/articles/using-pull-requests) GitHub model. Fork the PlayN project on GitHub, commit changes to your local fork, and submit a pull request with your changes.

Your changes will be reviewed on your fork, which may result in changes before they are accepted into the main repository. Because we don't want the back and forth history of cleaning up the changes in the main PlayN repository, it is best for you to add your changes in a branch in your repository. This allows you to update the changes based on review feedback and then simply abandon the branch once things are merged into the main PlayN repository.

### Bringing in new changes from the upstream repository ###
First you need to add a remote via which you will identify the upstream repository:
```
git remote add upstream https://github.com/threerings/playn.git
```

Now whenever you want to merge upstream changes into your clone, do the following:
```
git fetch upstream
git merge upstream/master
```