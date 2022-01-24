+++
title =  "Advanced Git Usage"
date =  2019-06-28T09:57:55+01:00
tags = ["Git","SDE"]
categories = ["tech"]
description = "Git is widely used. In some scenarios, more advanced skills are required. In this post, I will present some usages, which can impove our efficiency."
menu = ""
banner = "banners/windrad.JPG"
+++

## Introduction
Git works more than as a version controlling tool, it glues the teamwork together. It is widely used by almost all developers nowadays. Basic usages can be [git-cheatsheet](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)

However, in some scenarios, more advanced skills are required. In this post, I will present some usages, which can impove our efficiency.

## 1. git stash: store temporary dirty working branch

**Scenario: Interrupted workflow**
  When you are in the middle of coding, your boss comes in and demands that you fix something immediately. Traditionally, you would make a commit to a temporary branch to store your changes away, and return to your original branch to make the emergency fix, like this:

		$ git commit -a -m "WIP"
		$ git checkout bugfix/branch 
		$ ...edit emergency fix
		$ git commit -a -m "Fix in a hurry"
		$ git checkout my_original_branch
		# ... continue hacking ...

You can use git stash to simplify the above, like this:

		# ...doing your work until your boss comes...
		$ git stash
		$ edit emergency fix (in this branch or another branch)
		$ git commit -a -m "Fix in a hurry"
		$ git stash pop
		# ... continue hacking ...

[more reference](https://git-scm.com/docs/git-stash)


## 2. Retrieve committed code
you have committed code, but forget to push to the remote repo. After had checked out to another branch,
and changed back to previous branch, you found out that all the changes are gone. How to retrieve code? This 
method may apply to other scenarios.

		# to check the commit info and the abbrivation of commit-hashcode
		$ git reflog

		# to get the full information of logs. copy the hashcode
		$ git fsck --cache --no-reflogs

		# retrieve 
		$ git reset $(hash-code)
		
## 3. Overwrite complete a branch
the **production branch** regularly `completely compy` the latest verion of **dev branch**, and replace the old version(not merge) to avoid conflicts.
```
git checkout -B production_branch dev_branch 
```

## 4. git fetch vs. git pull

In most cases: 

git pull = git fetch + git merge

**git fetch:** When no remote is specified, by default the origin remote will be used, unless there’s an upstream branch configured for the current branch. You can retrieve new updates(newly created branch in remote repo)

**git pull** Fetch from and integrate with another repository or a local branch


# Reference
1. https://docs.microsoft.com/en-us/biztalk/technical-guides/planning-the-development-testing-staging-and-production-environments
2. https://blogs.technet.microsoft.com/devops/2016/06/21/a-git-workflow-for-continuous-delivery/
3. https://nvie.com/posts/a-successful-git-branching-model/

