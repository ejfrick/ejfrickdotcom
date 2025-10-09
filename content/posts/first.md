+++
draft = false
date = 2021-02-12T23:52:36-08:00
title = "First"
description = "First blog post"
slug = "Moving to Hugo in Docker"
authors = ["Enny J. Frick"]
tags = ["meta", "hugo"]
categories = ["meta", "blog", "hugo"]
externalLink = ""
series = []
+++
## I Switched to Hugo

My personal web presence has long run on Digital Ocean in a few forms, including as a Wordpress site and previously as a [Ghost](https://ghost.org) site. I hate Wordpress. I love Ghost.

That said, Ghost is really very heavy for what I need. Previously my website hosted just a copy of my resume and a contact form, and I realized I'd like to be able to lighten my resource requirements and consolidate various projects and services and stop using separate lightweight VPSes for everything.

Also, somewhat paradoxically, I wanted a little more customization out of my website without having to write front-end code. Again, I do love Ghost, but it feels like I run into exactly the wrong guardrails when I toy around with it.

Thus the switch to Hugo.

## I Dockerized my Website

I'm now running this purely static website in an Nginx Alpine Docker container! Currently, I build the content when merging to main on the GitHub repo for my site, build a container from it, and then push it to my Digital Ocean Docker repository from which I deploy and lifecycle the images.

This isn't really best practices; I may change this in the future to have the content be attached as a volume and then have the container load from there, or I may move to GitHub pages, or I may do neither of these things and run on AWS ECS Fargate and ECR or something similar.

## I Might Blog More

We'll see. I have thoughts on tech, whether I bother to write them down or not is anyone's guess, including mine. It'll effect whether or not I do the volume thing or not so!
