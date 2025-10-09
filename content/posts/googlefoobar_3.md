+++
draft = false
date = 2021-05-19T21:00:00-07:00
title = "Sometimes Brute Force is Optimal: GFB Pt 3"
description = "Google FooBar Part 3"
slug = "For List Key For List Key"
authors = ["Enny J. Frick"]
tags = ["code", "challenge", "google", "foobar"]
categories = ["foobar", "blog", "coding"]
externalLink = ""
series = ["Google FooBar 2021"]
+++
## Introduction

This is Part 3 in a [series](https://engjole.net/categories/foobar) about my experience with the Google FooBar challenge. Read the rest there, and if you don't want challenge spoilers, don't read on!

## The First Two-Part Level

For whatever reason, I decided to immediately request the next challenge after my fun prime time even though I was going to go on vacation relatively soon and thus had absolutely NO time to do anything! After requesting, I learned that this was the first level with two challenges, so to advance to the the fabled "level-before-you-consensually-give-Google-your-info" I'd have to do not one, but two tricky-dicky code things! Yay! But I did once again have 7 days to complete this challenge! Yay! But that meant doing this in the middle of the week somehow! Boo!

Fortunately, this challenge oddly seemed easier than the last: given a non-empty list of positive integers l and a target positive integer t, I needed to write a function that would verify if there is at least one consecutive sequence of positive integers within the list that can be summed up to the given target positive integer and returns the lexicographically smallest list containing the smallest start and end indexes where this sequence can be found, or returns the array [-1, -1] in the case that there is no such sequence. Seems simple, right?

## Here's Some More Fun Math

Thinking along the lines of the last problem, I figured my first approach would be to cheese this with some math instead of just brute force iterating through the list and checking all combinations of sequences. What Google was _really_ asking for was to check if the list contained a _partition_ of the given target number. A partition is basically a factor for addition, a combination of numbers that when summed, add up to the given number. Specifically, Google wanted a _strict partition_, a partition that contained only one instance of any given integer.

And because there is nothing new under the sun, my best friend the OEIS [even had a formula and sequence for the number of strict partitions for a given value of _n_](https://oeis.org/A000009), with _n_ being the index of said sequence! This formula is known as _Q(n)_. Has to be easy, right? All I need to do is get a list of strict partitions for a target integer, and then do some clever bisect match to find if there are any matches, and get the index range for those values!

## Math Isn't Fun

Wrong.

So while there are some optimal algorithms for calculating the _number_ of strict partitions for a given number, actually _finding_ them is another matter altogether. I was able to get some very acceptable runtimes for a function that got all strict partitions of _n_ where _n_ is less than or equal to 10, and some somewhat okay runtimes for where n is less than or equal to 50, but when n is bigger than that? Oof. Even _one_ iteration of the function took over 10 minutes! Taking forever to hand-implement memoization in Python 2.7 did not help that much either.

This is largely because values for _Q(n)_ grow exponentially with _n_, as do the number of regular partitions. I thought that the much smaller amount of strict partitions would let me have a reasonable chance at calculating strict partitions in the real world but alas, I was wrong!

## Spam For Loops and Enumerate

So instead I just iterated through all possible ranges in the list and stopped when the first range's sum was equal to the target value. And thanks to the enumerate function, I could iterate with the index and item at the same time, and just return the two indexes. And if the range was exhausted and there was no match, I'd just return the "no match" list.

Here's what my code ended up looking like:
{{<highlight py>}}
def solution(input_list, target_value):
    for start_key, start_value in enumerate(input_list):
        for end_key, end_value in enumerate(input_list):
            range = input_list[start_key:end_key + 1]
            if sum(range) == target_value:
                return [start_key, end_key]

    no_partition = [-1, -1]
    return no_partition
{{</highlight>}}
I try to avoid iteration whenever possible and just eliminate cases based on other criteria since iteration is typically costly but in this case there was absolutely no way around it without having code that took fifteen years to run through. Alas.

You can see the full code with comments [here](https://github.com/ENG-Jole/foobar.withgoogle/blob/main/numbers-station-coded-messages/solution.py)

## Conclusion

I feel like this challenge was a lesson in Occam's Razor: sometimes the simplest solution is the correct one. And while I am still confident in last challenge's use of Smarandache-Wellin number properties, I'm a little disappointed I didn't get to do something similar here, even though this answer is closer to how I code day to day: no tricky algos, just some very clean, simple code that gets the damn job done.

I'm on vacation next week so the next part will have to wait, but I'm curious to see what's next!
