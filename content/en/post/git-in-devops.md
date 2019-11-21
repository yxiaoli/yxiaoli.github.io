+++
title =  "Git tips in DevOPs workflow"
date =  2019-06-28T09:57:55+01:00
tags = ["DevOps","Git","SDE"]
categories = ["tech"]
description = ""
menu = ""
banner = "banners/IMGP5712.JPG"
+++

This is the second phase of my current project. After the Beta version of 
the project, I upgraded and shift the whole project into DevOps 
workflow. Git, is most essential part of the version control.
So in this post, I will shsare some tips that I have encountered

## Git Branching Models
There should be at least two models: `Dev` and `Master`. `Dev` branch is the front line defensing bugs. This branch
will proceed first round of testing. 
the `Master` or `Production` branch is used for release the stable version of the software. Still this branch will
go through all possible tests and report bugs.
   
![branch modes](https://app.yinxiang.com/shard/s33/res/d3525737-ac00-45cb-b4c2-98441aebb472/Screenshot%20from%202019-07-29%2014-26-40.png?search=Blog)

## Protect master branch 
In case the abuse of master branch, or any dangerous action to the release branch,
it is better to restricts the access to master branch.
when the feature
https://stackoverflow.com/questions/38864405/how-to-restrict-access-to-master-branch-on-git


## Create/ feature branch
git pull before your any actions
```console
git pull
git checkout -b new_branch current_branch
```
delete finished branch locally
```console
git branch -D wanted_to_delete

```

## Retrieve committed code
you have committed code / added all the code, but forget to push to the remote repo. After had checked out to another branch,
and changed back to previous branch, you found out that all the changes are gone. How to retrieve code? This 
method may apply to other scenarios.

```commandline
# to check the commit info and the abbrivation of commit-hashcode
git reflog

# to get the full information of logs. copy the hashcode
git fsck --cache --no-reflogs

# retrieve 
git reset $(hash-code)
```

## Overwrite complete a branch
the **production branch** regularly `completely compy` the latest verion of **dev branch**, and replace the old version(not merge) to avoid conflicts.
```
git checkout -B production_branch dev_branch 
```

# Reference
1. https://docs.microsoft.com/en-us/biztalk/technical-guides/planning-the-development-testing-staging-and-production-environments
2. https://blogs.technet.microsoft.com/devops/2016/06/21/a-git-workflow-for-continuous-delivery/
3. https://nvie.com/posts/a-successful-git-branching-model/

