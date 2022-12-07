---
title: "Argocd Starting Up"
date: 2022-11-30T08:48:47+07:00
draft: false
categories: ['technology', 'blog']
tags: ['technology', 'hugo', 'initial']
languageCode: 'en-us'
---

## Argo CD Starting up

Recently argocd is graduated from CNCF incubating project. 
we already using argocd in production almost 9 months. and its working awesome.

### Installation getting Started

to install argocd and setting this up you could easy follow this (argocd start)[https://argo-cd.readthedocs.io/en/stable/getting_started/]

## Lets Go

I will not explain how to install or how to configure argocd but i will share my experience of using it in production in our usecase.

> may be will be included what match and not match with our usecase and the workaround.

Argo CD is a **GitOps** Continuous Delivery Tools so your kubernetes yaml manifest will be on a repository and argocd periodicaly read and sync.
This git ops thing is awesome every push on our specific branch it will *auto update* manifest.

### First Problem is Semi Gitops

we implement argocd in the midle of rapid development. we cannot do full GitOps mechanism, Developers want a deployment flow that match their flow before. the dont want to commit the version of the image after build to do deployment.
so when pipeline running they want to see imidiate changes in their respected environment. 

thats when [Overides Parameters](https://argo-cd.readthedocs.io/en/stable/user-guide/parameters/) come handy

force parameters makes you able to changes the image tag (or any parameters) without changing the repository and do sync.

#### Pros :
- its allow imidiate changes by tooling. without changes the repository

    `argocd app set guestbook -p image.tag=abcd123` it will force argocd to change the image.tag to `abcd123`

#### Cons :
- the source of the truth is blury. because its save on the argocd not in the repository so we cannot say `whats in the repo is whats in the servers`

but as long as devs know what they are doing i think its fine to overides the parameters.

### Second Problem Standardization and Tooling

Standardization. i think this is problem in every company. we need to standardized how we store our manifest. we use multi repo and helm chart in every service is stored in service repositories. 

The workaround is we create a tools that makes devs easy to create the base helm chart and create argocd application manifest.
this tools also allow devs to build deploy and test in 1 straight command line.

`thetool syncimage ...` will sync the image in respected deployment.

this tools is help alot to standarize the command inside every pipeline

Thanks.



