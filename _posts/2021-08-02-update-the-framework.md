---
layout: post
title: Adjust/update/create code for our framework repo's.
subtitle: A guide on how to do it, so it becomes more fun to work on
author: Paulie
tags: [framework, updating, adapting, managing, git, syncing, fork, upstream, remote, merge]
comments: true
---

### Creating a fork
First things first. If you want to create new code or amend existing code it's always safe to work in your own environment rather than on the master files.
In order to do this you can create a Fork from the repository you want to work on. In this example we are working on https://github.com/mediamonks/temple

###If you already have a fork and want to update it
If you already have a fork, it is smart to first check if this fork is up to date with the current master repo.

##### Syncing a fork from the web UI
Go to the fork on git, my url is for example: https://github.com/mm-paulie/temple
Make sure you are logged in. Click `Fetch upstream` in the dropdown.
Review the details about the commits from the upstream repository, then click `Fetch and merge`.


#### Syncing a fork from the command line
Before you can sync your fork with an upstream repository, you must configure a remote that points to the upstream repository in Git. Open git bash in your forked temple folder type:
$ git remote -v

```  
> origin  git@github.com:mm-paulie/temple.git (fetch)
> origin  git@github.com:mm-paulie/temple.git (push)
```  

Specify a new remote upstream repository that will be synced with the fork.
```  
$ git remote add upstream https://github.com/mediamonks/temple
```  

Specify a new remote upstream repository that will be synced with the fork.
```  
$ git remote -v
origin  git@github.com:mm-paulie/temple.git (fetch)
origin  git@github.com:mm-paulie/temple.git (push)
upstream        https://github.com/mediamonks/temple (fetch)
upstream        https://github.com/mediamonks/temple (push)
```  

Now you can sync it by typing:
```  
$ git fetch upstream
```  

Check out your fork's local default branch - in this case, we use master:
```  
$ git checkout master
> Switched to branch 'master'
```  

Merge the changes from the upstream default branch - in this case, upstream/main - into your local default branch. This brings your fork's default branch into sync with the upstream repository, without losing your local changes.
```  
$ git merge upstream/master
```  

###Make your changes, do your magic

####But how do I know it works?
Use NPM link for testing your code. Create a new banner project using Yeoman:
```  
yo Richmedia Temple
```  
Go to your local forked Temple folder and type NPM link:
```
cd C:\git\m\mm-paulie\temple
npm link
```
At this moment, your local folder will be installed as the current temple package, you should see something like this after completion:
```
C:\Users\paulie\AppData\Roaming\npm\node_modules\@mediamonks\temple -> C:\git\m\mm-paulie\temple
```

Go back to your example project and type npm link:
```
cd C:\git\m\mediamonks\example_banner
npm link @mediamonks\temple
```
Then you should see something like:
```
C:\git\m\mediamonks\example_banner\node_modules\@mediamonks\temple -> C:\Users\paulie\AppData\Roaming\npm\node_modules\@mediamonks\temple -> C:\git\m\mm-paulie\temple
```
Now if you amend your forked temple it will reflect in your example project
Add and amend code, be sure to test it properly before you commit it. 

###Example project

I am going to make a fix for an issue on the generator-richmedia-temple.
https://github.com/mediamonks/generator-richmedia-temple/issues/88

First I use my own fork which I already had from the generator and I 'Fetch and merge' to the newest version.
Then I make sure that the generator-richmedia-temple, is actually connected to my local fork via npm link.


###Commit your changes
If you have a ticket/issue that will be closed with your amends, you can add to your commit title: `closes #xxx` and it will automatically be noticed that the ticket needs to be closed

push the commit in your own fork

create a new npm version patch/minor/major

Create a merge-request


###And now I want to unlink my local package?
First deinstall it from your test project
`C:\git\m\mediamonks\example_banner>npm unlink --no-save @mediamonks\temple'

Go to your local package, for example, my forked temple: `cd C:\git\m\mm-paulie\temple` and type:
`npm unlink`