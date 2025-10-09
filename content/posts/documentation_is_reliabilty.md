+++
title = "Technical Documentation is a Reliability Practice"
date = 2022-05-09T19:08:04-07:00
draft = true
description = "Why We Need to Pay More Attention to How and When We Write Docs"
slug = "technical documentation is a reliability practice"
authors = ["Enny J. Frick"]
tags = ["reliability", "sre", "documentation", "devops", "strategy"]
categories = ["blog", "reliability", "documentation"]
+++
## Introduction

Technical documentation[^1] is a reliability practice. Here's why.

## But First, Let's Talk About Toil for a Minute

The Great and Powerful [Google SRE Book](https://sre.google/sre-book/table-of-contents/) defines toil as "the kind of work tied to running a production service that tends to be manual, repetitive, automatable, tactical, devoid of enduring value, and that scales linearly as a service grows"[^2]. Unfortunately, I don't think we SREs tend to actually internalize this definition.

Instead, toil ends up being simply "work I don't like", which, of course, is unhelpful, because I could simply facetiously say "that's all work then". Let's call this _cowboy toil_. In some circles with slightly more mature engineering teams, toil ends up being "any work that isn't long-term engineering work". This, again, is unhelpful, since that captures hiring, performance reviews, and everything else that needs to get done as a matter of fact but which we don't typically capture in sprint planning; but it at least captures the importance of project work, which is essential. Let's call this _developer toil_.

I mention these unhappy definitions of toil because of a common factor: _both these definitions tend to askew documentation in favor of Getting Shit Done™_[^3]. That is, _documentation is very often considered toil_, either because operators/developers simply don't like to do it, or because it isn't what generates revenue for the company (namely, it's not either building cool new platform features or keeping the platform running).

## Let's Talk About What's Not Toil for a Minute

Let's go back to the SRE definition of toil for a second:

> Work that:
>
> 1. is manual
> 1. is repetitive
> 1. is able to be automated/autonomous
> 1. is tactical
> 1. is devoid of enduring value
> 1. scales linearly as a service grows

Let's see how documentation fits in here. Documentation certainly is manual; it often requires a few good hours fighting Confluence to get things to look remotely how one wants. It's also often repetitive; you can't just write docs once and leave them be, they require a continuous commitment. In some ways documentation is able to be automated, either through tools like PyDoc or by CI pipelines that generate rendered docs from raw Markdown to be placed in a wiki or a combination of both. Documentation can often seem tactical; it is often focused on reacting to situations or solving breaks in the system. Documentation can often also be devoid of enduring value; if no one reads the documentation or it's not readily accessible, or if it is made for services that will be deprecated "nExT QuArTeR", it feels like box-ticking rather than something useful. And documentation certainly scales linearly as the the service grows; as complexity and scale grows, so often do pages.

So documentation is toil then, right? It fits all the criteria?

Let's take another pass at this. Documentation is manual in the way code or commit messages are manual; sure, it requires human intervention, but so does clicking "Run with Parameters" in Jenkins. Documentation isn't repetitive; you can automate updates. Documentation can be automated, obviously. Documentation is strategic; good docs can mean the difference between a minute of downtime and ten minutes of downtime, and can prevent downtime from happening in the first place. Documentation has enduring value; it prevents large bus factors, speeds up on-call acceleration, and like I said earlier can prevent downtime from happening in the first place. Documentation doesn't scale linearly with service growth; once the core is established, deltas as growth occurs can be quite small.

[^1]: When I say _technical documentation_, in this context I am referring to _documentation relating to a technical system consumed by developers, operators, etc that both use and contribute to the development & operations of said system_. We can think of this as _internal documentation_ overall, documentation meant to be consumed by the same organization that produces it. This is in contrast to the broader scope of technical documentation, which includes external-facing documentation like user manuals.
[^2]: Vivek Rau, ed. Betsy Beyer, [_The Google SRE Book_](https://sre.google/sre-book/eliminating-toil/)
[^3]: not to be confused with Box's "Get Shit Done" mantra which refers to "not having meetings" rather than "we don't need docs"
