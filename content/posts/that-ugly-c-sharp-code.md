+++
draft = false
date = 2023-01-19T18:39:22-08:00
title = "That Ugly C# Code"
description = "Refactoring an internet meme"
slug = "That Ugly C Sharp Code"
authors = ["Enny J. Frick"]
tags = ["coding", "python"]
categories = ["blog", "coding", "python"]
externalLink = ""
series = []
+++

## Everyone's Favorite C# Code

If you're on the internet and are in any vaguely programming-oriented communities I'm sure you've seen [the really ugly C# sharp code](https://github.com/MinBZK/woo-besluit-broncode-digid-app/blob/ad2737c4a039d5ca76633b81e9d4f3f9370549e4/Source/DigiD.iOS/Services/NFCService.cs#L182) the Dutch government released recently. To save you the trouble, they're just if else-ing percentages to give a progress bar.

For whatever reason I thought it'd be fun to do a bunch of performance benchmarks in Python 3.10 to see if those silly Dutch programmers were onto something and to see how much sillier and faster I got.

## tl;dr

It's quicker than I would have wrote it, honestly.

I ended up writing 4 different implementations of the code on top of a Pythonized if-else version and the original implementation was pretty performant comparatively.
