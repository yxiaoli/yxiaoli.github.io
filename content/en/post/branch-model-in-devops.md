+++
title =  "Three-Stage Branch Model applied in DevOps - CI part"
date = 2020-07-29T15:06:14+02:00
tags = ["DevOps", "Git", "Operation", "Strategy", "Microservices"]
categories = ["tech"]
description = "I am working on a data platform project, the product itself is composed of several microservices. Our team is under great pressure of operation and mantainance. Thanks to the DevOps helps us keep the balance of development and deployment."
menu = ""
banner = "banners/spark-multi-nodes.jpg"
+++

I am working on a data platform project, the product itself is composed of several microservices. Our team is under great pressure of operation and mantainance. Thanks to the DevOps helps us keep the balance of development and deployment.

In this post, I illustrate how we maintain the CI(continuous integration) part, which contains: the branch strategy we adopt, the concept of DevOps, overview of the workflow, the tips.

next post, I will continue to explain the CD part(cotinous deployment)

## 1. Overview: 3-stage Branching Model

There should be at least two long-live branches: `Dev` and `Master`. `Dev` branch is the front line defensing bugs. This branch
will proceed continuous integration and testing. All the feature, hotfix, customized branches should firstly be merged to `Dev` branch.
The `Master` or `Production`(short for prd) branch is used for releasing the stable version of the product. In some scenarios, we need a `Int` (abbr. Integration) branch, between `Dev` and `Prd` branch. We keep 3 long-live branches: Dev, Release, Prd

- Dev: this branch accepts all the PR from feature branches, and unit tests will be done in this branch.
- Int: this branch accepts release PR
- Prd: this branch accepts stable, UAT PR


## 2. DevOps Concept

There are three concepts which I think we should keep in mind if we want to be good DevOps player.

- Agileness: normal we apply DevOps together with the modern software practice: Scrum. We break down the requirements into smallers features. Instead of giving a breaking change at once after a long period of time, DevOps helps incrementally develop and build the product frequenctly. 

- Tracibility: we use `JIRA` to trace the progress in each sprint. Starting the work by creating a feature branch from the `JIRA` ticket. After the feature branch has been merged, we can close the ticket.

- Automation: by using configuration, scripts, and tooling we enabled to sync, compile, build automatically. The automation of the process eliminates repetitive manual work and human error. It vastly improve the efficiency.

## 3. Workflow 

step 1:  start the sprint, developers work on feature branches which are created from DEV

step 2: finish the development then merge to the DEV.

step 3: unlike azure environment, on-prem we must merge to INT if we want to test in the runtime environment

step 4: sprint ends around the corner, we finish all the tasks and merge to the DEV.

step 5: create a release branch from dev:  for example V0.2.3  (0 means: year;  2 means: PI;   3 means : sprint)

step 6: start to test in INT, found a bug

step 7: create a bugfix branch, merge to DEV first(make sure the main branch has the latest fix), then to release, INT

step 8:  the Integration Test ends which stands for the success of the release

step 9: after the PI, we can roll out on PRO


![branch modes](https://app.yinxiang.com/shard/s33/res/e89fe792-7189-4fe5-b1da-f6c62b94646b/workflow%281%29.png?search=blog%20image)


## Create a feature branch
git pull before your any actions
```console
git pull
git checkout -b new_branch current_branch
```
delete finished branch locally
```console
git branch -D wanted_to_delete

```


## 4. Tips 

1. Protect master branch 
In case the abuse of master branch, or any dangerous action to the release branch,
it is better to restricts the access to master branch.
when the feature
https://stackoverflow.com/questions/38864405/how-to-restrict-access-to-master-branch-on-git

2. commit frequently

3. create PR and discuss with PR reviewrs eariler

4. merge to dev and automatically trigger the deployment


# Reference:
1. [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)

