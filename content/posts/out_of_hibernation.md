+++
draft = false
date = 2025-10-09T12:42:28+01:00
title = "Out of Hibernation"
slug = "out-of-hibernation"
authors = ["Enny J. Frick"]
tags = ["meta"]
categories = ["meta", "blog"]
externalLink = ""
series = []
+++

## I Am, Yet Again, Still Alive
Remember what I said [last time](/posts/new_year_new_url/) about yearly blog posts? Well, here's the annual smattering of updates!

## Progressing Past The Need For `sed`
I finally got around to upgrading `hugo` and the modules for this site, and as part of it I very much wanted to get off my gross `sed` command I was using to set deployment configuration as part of the site's pipeline.
`hugo` does support using [environment variables](https://gohugo.io/configuration/introduction/#environment-variables) to configure sites instead of using a config file, so I was very, _very_ much hoping that I could just...do that.
Alas, after some futzing it turns out that since that deployment targets are _slices_, and not _maps_ (thanks TOML!) in the config, it was not actually possible.
Somewhere, the inventor of the twelve-factor application paradigm is rolling in their grave.

Then, of course, I remembered that `envsubst` was a thing, so...I am just doing that. Yeah. It's truly just a matter of time before I create my deployment tool instead of using `hugo deploy`.

## I Have No Idea Where 2024 (Or 2025) Went
For obvious reasons.

I have been up to a _lot_ in the past year plus in terms of work and life, and I will eventually maybe write about it! We'll see!

## I Live In London Now
Yes, _that_ London. I very much miss Portland and I don't think I will ever get used to beans on toast.

## My Public Keys Are Now Available
And you can find them [here](/dev/keys/).

One day I will get around to adding projects on here too! Maybe next year!
