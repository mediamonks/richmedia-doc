---
layout: post
title: Adjust/update/create code for our framework repo's.
subtitle: A guide on how to do it, so it becomes more fun to work on
author: Paulie
tags: [framework, updating, adapting, managing, git, syncing, fork, upstream, remote, merge]
comments: true
---

# Note: This is still a wip  version of the post

### Creating a fork
First things first. If you want to create new code or amend existing code it's always safe to work in your own environment rather than on the master files.
In order to do this you can create a Fork from the repository you want to work on. In this example we are working on https://github.com/mediamonks/generator-richmedia-temple
![Screenshot create a fork](./assets/img/update_the_framework_create_a_fork.gif)


### If you already have a fork and want to update it
If you already have a fork, it is smart to first check if this fork is up to date with the current master repo.

##### Syncing a fork from the web UI
Go to the fork on git, my url is for example: https://github.com/mm-paulie/generator-richmedia-temple
Make sure you are logged in. Click `Fetch upstream` in the dropdown.
Review the details about the commits from the upstream repository, then click `Fetch and merge`.
![Screenshot create a fork](./assets/img/update_the_framework_fetch_andmerge_via_ui_example.gif)

#### Syncing a fork from the command line
Before you can sync your fork with an upstream repository, you must configure a remote that points to the upstream repository in Git. Open git bash in your forked temple folder type:
```
$ git remote -v
```  

And you will see something like this:
```  
> origin  git@github.com:mm-paulie/generator-richmedia-temple.git (fetch)
> origin  git@github.com:mm-paulie/generator-richmedia-temple.git (push)
```  

Specify a new remote upstream repository that will be synced with the fork.
```  
$ git remote add upstream https://github.com/mediamonks/generator-richmedia-temple
```  

After connecting the upstream, when you type $ git remote -v you should see this:
```  
$ git remote -v
origin  git@github.com:mm-paulie/generator-richmedia-temple.git (fetch)
origin  git@github.com:mm-paulie/generator-richmedia-temple.git (push)
upstream        https://github.com/mediamonks/generator-richmedia-temple (fetch)
upstream        https://github.com/mediamonks/generator-richmedia-temple (push)
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


#### How do I know it works?

Use NPM link for testing your code. NPM link gives you the ability to connect a local (test) repository with your current project instead of the online repository that by default is connected.

Clone your forked generator-richmedia-temple to a local folder:
![Screenshot clone](./assets/img/update_the_framework_clone.gif)


Since we are testing with the generator, we need to make sure it uses our local generator-richmedia-temple folder files even before we generate a banner project.

Go to your local forked generator-richmedia-temple folder and type NPM link:
```
cd C:\git\m\mm-paulie\generator-richmedia-temple
npm link
```
At this moment, your local folder will be installed as the current generator-richmedia-temple package, you should see something like this after completion:
```
C:\Users\paulie\AppData\Roaming\npm\node_modules\@mediamonks\generator-richmedia-temple -> C:\git\m\mm-paulie\generator-richmedia-temple
```
![Screenshot npm link](./assets/img/update_the_framework_npm_link.gif)

Add something to your local forked generator-richmedia-temple
We are going to add the meta tag as described in the issue
https://github.com/mediamonks/generator-richmedia-temple/issues/88

![Screenshot npm link](./assets/img/update_the_framework_edit_generator_file.gif)

Change to a new project folder
![Screenshot npm link](./assets/img/update_the_framework_show_change_0.gif)

```
cd C:\git\m\mm-paulie\test
```

Create a new banner project using Yeoman:
```  
yo richmedia-temple

```  

In this case it will actually use the generator that is locally on your computer and you will see the change in the template
![Screenshot npm link](./assets/img/update_the_framework_show_change_0.gif)

### Commit your changes
If you have a ticket/issue that will be closed with your amends, you can add to your commit title: `closes #xxx` and it will automatically be noticed that the ticket needs to be closed

push the commit in your own fork

create a new npm version patch/minor/major

Create a merge-request


### And now I want to unlink my local package?
First deinstall it from your test project
`C:\git\m\mediamonks\example_banner>npm unlink --no-save @mediamonks\generator-richmedia-temple'

Go to your local package, for example, my forked temple: `cd C:\git\m\mm-paulie\generator-richmedia-temple` and type:
`npm unlink`