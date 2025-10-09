+++
draft = false
date = 2021-12-01T21:00:00-08:00
title = "Advent of Code 2021: Day 1"
description = "Advent of Code '21 Day 1"
slug = "Advent of Code 21 Day 1"
authors = ["Enny J. Frick"]
tags = ["code", "challenge", "advent of code", "advent of code 2021"]
categories = ["advent of code", "advent of code 2021", "blog", "coding"]
externalLink = ""
series = ["Advent of Code 2021"]
+++

## Wait, I Thought You Didn't Like Coding Challenges

I mean...I don't, really. I am definitely not into competitive programming or tricky dick algorithm optimization to get that .01% less CPU usage because somehow the standard library of the language, which is often about as fast as you're going to get if there's compiler optimization going on, is somehow not fast enough. [^1] [^2] [^3]

[^1]: I guess this is mainly a Python complaint since the stdlib is mainly written in C and thus will forever be faster, but I don't see people doing LeetCode in Rust or C 😉

[^2]: But also, if you need REALLY performant Python code you're not going to beat a [JIT](https://www.pypy.org) or [Cython](https://cython.org), and is there even a use-case where you have to use highly optimized highly performant vanilla Python where you can't use  JIT'd Python or Go or Rust or Kotlin or C or any other more appropriate for the job programming language?

[^3]: I digress

But you're right, dear reader, I don't like LeetCode-style competitive programming challenges. Advent of Code is definitely not that though! For one, they're all fairly short problems and don't require you to retake Data Structures and Algorithms to be good at them, and for two, they focus on _general programmatic problem solving_, the most important part of developing, rather than _hey what algorithm is this and implement it in the most optimal way possible_. This is to say: I rather like them! They're fun! They're puzzles! They scratch my itch for coding and don't require me to go on the OEIS every time! So I'm gonna (hopefully) do it this year, mmkay?

## Day 1 Spoilers

### The Problem

Given a list of ints, determine how many times the nth + 1 int is greater than the nth int.

### The Solution

So ok, I'm lying slightly about the problem... you're not actually given a list of ints. Hell, you're not given anything other than a webpage! It is entirely up to you to grab the dataset and convert it into the format you need, which, ok, no big deal, let me just install requests real quick, grab that URL, convert it to text, split it into a list, map int to each elem and...realize I need a session cookie or auth token because the input is dynamically generated. Alright. Let's try this again.

Given a webpage? Easy! Install requests, grab that url, put the session ID in my environment, grab that from the environment, pass it as a dict to requests.get, convert the results to a string, split that string, and list comp it so it's actually ints! Yay!
{{<highlight py>}}
SESSION_ID = os.environ["AOC_2021_SESSION"]
<!-- markdownlint-disable-next-line -->
URL = "https://adventofcode.com/2021/day/1/input"

def url_to_list_of_ints(url: str) -> list[int]:
    list_of_ints = [
        int(item)
        for item in requests.get(url, cookies={"session": SESSION_ID}).text.split()
    ]
    return list_of_ints
{{</highlight>}}
There we go! An actual list of ints! What's next?

So it's very obvious that we need to iterate over the list and do things with each value. Now, the tricky thing is is that a lot of the times you want to do something with an index of a list, you don't actually want to use ```for index in range(len(list))``` to iterate over the indexes themselves, since a lot of the times you're just immediately going to get the value for that index anyway... this is why you use ```for index, value in enumerate(list)``` instead if you need the "every nth value" or whatever. Or, if you are trying to iterate over two lists the same length and use the index as a placekeeper, you instead do something like ```for av, bv, in zip(list_a, list_b)``` instead. But this time, I actually DID need the index, since I was looking ahead for the value after the one I was on and doing an if check on the difference between the two values:
{{<highlight py>}}
def get_num_of_larger_measurements(measurements: list[int]) -> int:
    larger_measurements = 0
    for index in range(len(measurements) - 1):
        if measurements[index + 1] - measurements[index] > 0:
            larger_measurements += 1

    return larger_measurements
{{</highlight>}}
I was then just cumulatively adding each greater than 0 difference to the pseudo-counter I set up. Worked out ok. I probably could have instead used some itertools trickery for the lookahead but I think this is generally more readable...and it's not like performance really matters! This passed successfully and....

### Wait, There's a Part 2????

Given a list of ints, get the number of times the nth + 1 sum of the three item sliding window is greater than the nth sum of the three item sliding scale window.

### The Solution (Again)

Well, despite being a little confusingly worded, this one isn't that bad at all. Looking at the example shows it a little better:
{{<highlight text>}}
199  A
200  A B
208  A B C
210    B C D
200  E   C D
207  E F   D
240  E F G
269    F G H
260      G H
263        H
{{</highlight>}}
For each letter triad, sum the numbers together. Then, given the list of sums, determine how many times the nth + 1 sum is greater than the nth sum. So, all I needed to do was get the sliding scale sums and then run the same larger measurement function against that. Here comes our ugly friend ```for index in range(len(list))``` again:
{{<highlight py>}}
def get_sliding_scale_num_of_larger_measurements(measurements: list[int]) -> int:
    sums = []
    for index in range(len(measurements) - 2):
        sums.append(
            measurements[index] + measurements[index + 1] + measurements[index + 2]
        )

    larger_measurements = get_num_of_larger_measurements(sums)
    return larger_measurements
{{</highlight>}}
And again, this passed successfully.

## Conclusion

Overall I quite enjoyed my experience! I didn't ever feel stuck and I was able to knock it out over lunch. I know there's varying difficulty so I'm looking forward to some tricker ones, but I really do think these hit a nice space where you can have creative freedom and be fluent in the language you choose without needing to retake DS&A :)

[All code in full is in GitHub](https://github.com/ENG-Jole/advent-of-code-2021)
