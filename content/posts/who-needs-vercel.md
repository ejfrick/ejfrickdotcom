+++
draft = false
date = 2022-06-29T23:22:04.000Z
title = "Building Hugo Previews on PRs"
description = "Using GitHub Actions to do the job of a CMS"
slug = "who needs vercel anyway"
authors = [ "Enny J. Frick" ]
tags = [ "github actions", "cicd", "hugo" ]
categories = [ "blog", "devops", "hugo", "hunge labs" ]
externalLink = ""
series = [ "Hunge Labs" ]
+++

## Intro

I recently started contributing to [Hunge Labs](https://github.com/hungelabs), an open-source specialty coffee knowledgebase/wiki/project started by [Tess Skye](https://github.com/TesseraSkye) who I know from the James Hoffmann Patreon Discord. Long story short I convinced Tess that the site should be moved from Jekyll to Hugo (since Jekyll and Ruby are both gross) and did [allllll that work](https://github.com/hungelabs/hungelabs.github.io/pull/1) porting stuff over. Story of my life short (aka I have a propensity to overdo things), I set up a roadmap for an MVP of the wiki and have been chipping away (and trying to enlist others to chip away) at our issues in between doing things at work that maybe will be separate blog posts and doing things in life that will not be separate blog posts. One of the things I wanted to do to ensure the contributor & developer experience was a-ok was to build previews of each PR to validate both content and backend changes.

### Wait, Why Not Just Use a CMS Instead of an SSG?

Because GitHub Pages is free, I'm tired of managing servers in my free time, and because most of us coffee folks are nerdy enough to handle writing some Markdown and doing some git commits.

## Enter... Vercel

[Vercel](vercel.com) is the obvious answer when it comes to building front-end preview environments; it is, in fact, designed for this, and unlike GitHub Pages doesn't require any finagling to use a framework that is not named Jekyll. So, I went to work setting it up, getting the appropriate org perms to install the Vercel app in GitHub, linking the repo, wondering why the hell it defaulted to an ancient version of Hugo...

Wait, what? Why does it use a two year old version of Hugo? Ok, sure, you can set the version by specifying an envvar but that is not a super obvious solution...and also makes no sense. Aren't most people who are going to set up Vercel from scratch going to do so for a new project, one that would use the latest version of Hugo because that's what they downloaded? Anyway, that weirdness aside, once I got the correct version of Hugo set up for Vercel...the site still wouldn't build. Ugh.

You see, Hugo smartly swapped from using git submodules to handle themes to using basically Go modules, similar to what Jekyll does with Ruby gems. And say what you want about Go modules, but they are a hell of a lot easier to deal with in CI than git submodules. Ugh, so tired of having to update git submodules and building tools for that. The only problem with this is you need Go installed to interact with Hugo modules. This wasn't a big deal when setting up GitHub Actions; I think either the runner by default had Go installed or the action I use to build Hugo installs it. Regardless, it's there when need it...which is more than I can say about Vercel. I ended up having to do something like `yum install go1.14 && hugo --minify` for my Vercel build command, which, honestly? Kinda gross. I guess I could have done it in the install step possibly but I had heard weird things can happen.

This Vercel experience was turning out to be unpleasant. For a product with a typically great developer experience, it was frustrating to realize that the implementation of a supported framework was basically half-assed; legacy version by default without the toolchain needed for the current version installed. But, post-Go installation, I was finally able to deploy the site to Vercel.

And that's when things went from bad to worse.

You see, since the site repo is owned by a GitHub Organization, Vercel forcibly created a team account for it. Which, fine, billing/free tier issues aside (a problem for future me), means that _you can't create access tokens under the team account_. You can create an access token under a user account and scope it to the team, but that token is ultimately owned by the user--so if they leave your company or project, you're screwed. And to create a fake bot user means you're paying more money for a member slot for a feature that should just be included. Sure, this is irrelevant if you just use the GitHub app instead of the command line or a GitHub Action, but that means you have to build a preview for every commit. There isn't exactly an easy way to come up with some fancy git filter to say "only build PRs" since that's a GitHub rather than git feature. So if you want literally anything else other than a preview on every commit save for pushes to a specific branch, and you have a GitHub org, you're SOL.

## Enter... Some GitHub Actions

With Vercel DOA, I wondered if I could just do some GitHub Pages trickery and deploy the previews there. And turns out...yep! You can! As usual, someone had already solved this problem: [Deploy PR Preview](https://github.com/marketplace/actions/deploy-pr-preview) has been around for a bit and does all the hard work of generating per-PR previews of whatever static site you have; just bring your own buildchain and tell it where to deploy from. I immediately liked this better than Vercel. I could just recycle the build tooling for the production wiki (and from this site 😉) and only needed to configure the preview deployments themselves. Additionally, I found the URL generation to be a little nicer; instead of randomly generated URLs like Vercel gives you,. Deploy PR Preview throws previews under an umbrella folder and the PR number. A lot easier to know what's what! After changing the production deploy workflow to use a different action for deployment so I didn't need to worry as much about conflicts, I created a workflow for PR previews and.... it didn't work.

Instead of deploying to `hungelabs.github.io/preview/PR_NUMBER`, the action was deploying to `hungelabs.github.io/hungelabs.github.io/preview/PR_NUMBER` and thus just redirected back to prod. Whoops. This is a quirk on the URL pattern for GitHub Pages sites. There are two types of sites, project sites and org/user sites, with slightly different naming requirements for each. Essentially org/user sites live at the root of a org/user's GitHub Pages domain (i.e. `ORG.github.io`) while project sites are under a folder (`ORG.github.io/SITE`). The action only generated URLs that matched the project site pattern, so org/user sites just didn't work.

I hemmed and hawed a bit about whether to override envvars or create a copy of the action inside the site repo and ultimately did the proper open source thing of forking the project, fixing the issue (with copious use of `cut`) and creating an upstream PR after verifying it worked for the wiki. Yay!

## Vercel Kinda Sucks

That is honestly my takeaway. Vercel does one thing well--provide both prod and preview deployments for sites using non-static frameworks (aka Next.js), but tends to make things difficult if you step outside the mold. This is not to say tools shouldn't be opinionated; they should be. But if you're going to be opinionated, _don't pretend to support patterns that fall outside the opinion_. I will probably use Svelte (which I honestly really enjoy) along with Vercel next front-end thing I do, but for Hugo-world, at least, I am staying away from Vercel and learning to love GitHub Actions more.
