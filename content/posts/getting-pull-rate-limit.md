---
title: "Getting Pull Rate Limit and Workaround"
date: 2022-12-13T12:04:51+07:00
draft: false
author: "prima adi"
categories: ['technology', 'docker', 'kubernetes']
tags: ['technology', 'docker', 'kubernetes', 'workaround']
languageCode: 'en-us'
---

# When docker hub want you to authenticate

there is a stable system using public images from docker hub. and suddently pull rate limit error is showing up.

![Rate Limit Error](/img/getting-pull-rate-limit-3.png)

## This is How we implement the Workaround

Our system is running in AWS EKS and using all internal image registry (ECR) for application images. Most of application is using docker images and run's on kubernetes.
but some POC and small services is using public images from docker hub. it needs you to authenticate or login to be able to increase the limit.

> i am not really aware how much is the limit

But with this problem there is conclusion that we need *near no limit* image registry so there is no problem such rate limit pull request.

### Oprtion 1 : Internal common registry

For most of companies this approach is pretty easy, you just need to create one more ecr that holds your common images.

but in our case is little different, we are multiple account aws architecture. if we want to make central image registry we need to create and copy it to multiple account. and besides our common images is actually all (could public) images.

### Option 2 : There is actually AWS public Images.

So if you are ok with it. (no common image that use a secret or specific product usecase). you could actually push your common docker image to aws public repository 

- [Creating Public Repository](https://docs.aws.amazon.com/AmazonECR/latest/public/public-repository-create.html)
- [Push Image to Public repository](https://docs.aws.amazon.com/AmazonECR/latest/public/docker-push-ecr-image.html)
- [Search on Gallery Of public Repo AWS](https://gallery.ecr.aws/)

with this approach we could actually reach our goal to share common image accros orgs with more limit

### Comparison AWS ECR Public Image and Docker Hub Image (anonymous)

for AWS ECR Public Repository [Docs Limit Here](https://docs.aws.amazon.com/AmazonECR/latest/public/public-service-quotas.html) we have 10 pulls per second

![Rate Limit AWS Public ECR](/img/getting-pull-rate-limit-1.png)

and for docker un-Authinticated pull, you only have 100 per 6 hours

![Rate Limit AWS Public ECR](/img/getting-pull-rate-limit-2.png)

in our usecase we need the *Option 2* approach by the way.
