+++
draft = false
date = 2021-05-14T00:30:00-07:00
title = "Turns Out I Know A Lot About Primes Now: GFB Pt 2"
description = "Google FooBar Part 2"
slug = "Wow How Many Times Can I Say Smarandache-Wellin Number?"
authors = ["Enny J. Frick"]
tags = ["code", "challenge", "google", "foobar"]
categories = ["foobar", "blog", "coding"]
externalLink = ""
series = ["Google FooBar 2021"]
+++
## Introduction

This is Part 2 in a [series](https://engjole.net/categories/foobar) about my experience with the Google FooBar challenge. Read the rest there, and if you don't want challenge spoilers, don't read on!

## 7 Days to Do Stuff With Primes

Reading some background on GFB, I found out that the first challenge used to be a lot shorter! For this challenge, I had seven days after receipt to write a function which takes in the starting index _i_ of a concatenated string of all primes, and returns the next five digits in the string! Previous first challenges were only 48 hours long, and for this challenge, I think 7 days is a fair period. While it seems deceptively easy at first, since you're just grabbing a index from a string than can be calculated, it is much much harder to do properly, since you must account for all positive values of _i_ less than or equal to 10,000!

## Three Prime Approaches

The way I saw, there were three approaches to this problem:

First, you could generate a sufficiently long concatenated string of primes, as optimally as possible, and then just get the index from there. However, if your index is 10, you just generated a shitton of primes for nothing and wasted a lot of resources. So, overall, easy, guaranteed to work and be implementable, but unless you're running it on a supercomputer each iteration is going to take its sweet time generating a bunch of primes and is just very very inefficient.

Second, you could take the index and generate a string of primes that is _only just_ longer than the index, say _i_ + 5 primes. This represents a massive massive performance improvement to the first approach, especially with a tricky dick algorithm, but still means generating more primes than you actually need.

Third, you could just exploit some property of primes to generate only the part of the string of primes that the index plus five was contained in. This also is a primes-on-demand approach, but is much much much more efficient, since no primes are "wasted" in your calculation. And this, dear reader, is the approach I immediately thought of.

## The Prime-Counting Formula and Smarandache-Wellin Numbers

Having watched way too much Numberphile, I knew there was probably some property of primes I could use to arbitrarily calculate which nth prime would have what length. I decided I would treat the index not as an index, but as the _total length_ of an arbitrarily long string of primes. So if I could find out  how the string of primes grew as a function of the number of primes concatenated, I could then reverse this formula to give me the number of primes for a given length. After that I could just calculate a few more primes beyond that, and then get the five digits after the index by only calculating a few primes total, or by at least knowing when to stop calculating exactly.

I ended up at the ever-useful [Online Encyclopedia of Integer Sequences](https://oeis.org) and found out that this arbitrarily long concatenated string of primes is actually a special class of number called a _Smarandache-Wellin number_! From there, I began searching for formulas for calculating the length of these, since as the _nth_ SWN approaches infinity its rate of growth increases per the prime number theorem, and came up with...nothing. For SWNs, at least.

But, since SWNs are concatenated primes, their rate of growth is effected by the rate of growth of the length of prime numbers! I ended up eventually at a different OEIS entry, this time for _the sequence of the number of prime numbers with digits < m_, _m_ being the index of the sequence. This meant that from this series, I could determine how many digits the _nth_ prime would have, and then recursively calculate the total length of the _nth_ Smarandache-Wellin number! From there, I would just need to solve for the _nth_ SWN (which is also the _nth_ prime in the sequence) as a function of the total length!

## Creating an Algorithm for the Length of a Smarandache-Wellin Number

First, I ended up creating a specific version of a SWN length algorithm using the aforementioned _number of primes < m_ sequence as bounds for a piecewise function:
{{<highlight py>}}
# For the nth SWN, return the total number of digits
def swn_length(n):
    if n == 0:
        length = 0
    elif 0 < n <= 4:
        length = n
    elif 4 < n <= 25:
    <!-- markdownlint-disable-next-line -->
        length = 4 + 2 * (n - 4)
    elif 25 < n <= 168:
    <!-- markdownlint-disable-next-line -->
        length = 46 + 3 * (n - 25)
    elif 168 < n <= 1229:
    <!-- markdownlint-disable-next-line -->
        length = 475 + 4 * (n - 168)
    elif 1229 < n <= 9592:
    <!-- markdownlint-disable-next-line -->
        length = 4719 + 5 * (n - 1229)
    else:
        raise ValueError
    return length
{{</highlight>}}
This worked blazingly fast, since it was only doing simple math! However, I disliked the inelegance, and verbosity, so created a generalized formula for the length of an SWN:

> For the nth SWN, given the set of number of primes P with at most m digits where m is the index of the set, the length is as follows:
> for P[m-1] < n <= P[m]:
> L(n) = L(P[m-1]) + m(n - P[m-1])

Beautiful! Seems much more functionally oriented. I went to work writing it in Python:
{{<highlight py>}}
# a006880 is the list of the number of primes < m
def swn_length_iterative(n):
    length = None
    for index in range(0, len(a006880) - 1):
        if n == 0:
            length = 0
        if a006880[index - 1] < n <= a006880[index]:
            length = swn_length_iterative(a006880[index - 1]) + index * (n - a006880[index -1])
            break
        else:
            continue

    return length
{{</highlight>}}
Much easier to see what's acutally going on here, and seems less arbitrary than a bunch of if than's and numbers. However, on comparing these two algorithms, since the latter was a for loop, and Python sucks sometimes, I realized my fancy generalized algo was 23 times slower than the hardcoded one, so hardcoded won out. Plus, it made it a lot easier to make a specific algo to get the _nth_ prime from the length:
{{<highlight py>}}
def check_length(length):
    if length == 0:
        n = 0
    elif 0 < length <= 4:
        n = length
    elif 4 < length <= 46:
        n = (length + 4) / 2
    elif 46 < length <= 475:
        n = (length + 29) / 3
    elif 475 < length <= 4719:
        n = (length + 197) / 4
    elif 4719 < length <= 46534:
        n = (length + 1426) / 5
    return n
{{</highlight>}}

I did something similar to my optimized SWN algo to get the length of individual _nth_ primes. For all of these algos I stopped at the next mark past an index of 10,000 for my bounds.

## Converting String Indexes into SWN Lengths

Something I noticed about my nth-as-a-function-of-length algo is that when I was passing indexes to it, not all indexes would return an integer value of _n_ when using a real calculator! Thanks Python floats and computer division! You obviously can't have the 2.5th prime, so I need to find some way to deal with that, since I was just going to pass raw index values to it, and somehow remember where the index was in relation to the valid length of the calculated _nth_ SWN.

I implemented a function to turn the index into a _possible nth prime_ value, checked to see if it was a valid _nth_ prime, and checked the delta from there. I ended up using a function I had written earlier to get the necessary amount of primes to get five digits after any index of the _nth_ prime to create a spread of _n_ values for possible candidates if the possible prime wasn't valid, and used a bisect-based comparison function to get the closest valid _n_ value. I then subtracted the raw string index value from SWN length of the closest _n_ value to get a delta from which I could base the five digits off of. This code ended up being a pain in the ass to get working properly; between certain lengths there was drift of one or two places as compared to the brute force result and I ended up calculating the length of the _nth_ prime and adding that value minus one to the delta to compensate for this drift. Here's what the final code looked like:
{{<highlight py>}}
def nth_from_index(string_index):
    poss_n = check_length(string_index)
    if swn_length(poss_n) == string_index:
        prime_start = poss_n
        init_position = 0
    else:
        spread = get_necessary_prime_amount(poss_n)
        delta = int(ceil(spread / 2.0))
        lower_bound = poss_n - delta
        upper_bound = poss_n + delta + 1
        prime_dict = {}
        for m in range(lower_bound, upper_bound):
            prime_dict[swn_length(m)] = m
        prime_index = take_closest_rd(
            sorted(list(prime_dict.keys())), string_index)
        prime_start = prime_dict.get(prime_index)
        init_position = string_index - prime_index

    length = lenprime(poss_n)
    position = init_position + length - 1

    return prime_start, position
{{</highlight>}}
I think this could probably could have used some optimizing but I thought it was good enough and during my own testing worked for all expected values of the index.

The rest of the code was rather straightforward, some helper functions for the creation of a concatenated list of primes, and some islice uses to chop up said list at the appropriate position. For prime generation I ended up using an unbounded sieve of Eratosthenes generator and just next'd and islice'd the _nth_ prime I needed. The main solution function took an index, got a nth prime and delta out of it from my nth_from_index function, got the next _x_ primes from the _nth_ prime, and then cat'd them all together and sliced out what it needed.

You can see my final code [here](https://github.com/ENG-Jole/foobar.withgoogle/blob/main/re-id/solution.py)

## Conclusion

I ended up submitting my solution after working on it for four calendar days, but only spent maybe 2 full evenings after work working on it. I'm quite happy with my end result, especially since I had to write everything in Python 2.7! This meant no stdlib LRU caching, which I think could easily speed up the prime generation, especially during benchmarking for all accepted values of index.

I'm excited for the next part of the challenge, and will hopefully make it through all five!
