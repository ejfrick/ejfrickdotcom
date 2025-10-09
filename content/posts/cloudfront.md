+++
draft = false
date = 2021-06-04T22:30:00-07:00
title = "I Once Again Moved My Website"
description = "Going fast and smooth"
slug = "Moving to Hugo on S3"
authors = ["Enny J. Frick"]
tags = ["meta", "hugo"]
categories = ["meta", "blog", "hugo"]
externalLink = ""
series = []
+++
## Moving to CloudFront and S3

Remember how I said the Docker container method wasn't the best way to run this site?

I was absolutely right. I had spent enough time looking at ```hugo deploy``` while writing the Foobar posts I realized it'd be super easy and probably cheaper just to move everything to a bucket with CloudFront in front of it...and after looking at the price calculator I was right. Damn, S3 is cheap. So, here we are.

To do this, I created an S3 bucket and CloudFront distro, added the deploy info to my ```config.toml```, gave it a test run, and was happy with how smooth it was. I then updated my GitHub actions workflow to set up AWS creds and treat the S3 and CloudFront IDs as a secret, committed, watched as my build failed because of a sed delimiter thing, fixed that, and then woo! CI/CD to S3.

I ran a ping test against the old Docker server and the CloudFront distro while I had both up:

New:
{{<highlight bash>}}
<!-- markdownlint-disable-next-line -->
    ❯ ping -c 5 www.engjole.net
    PING doi5jqyokxnhv.cloudfront.net (54.192.73.49): 56 data bytes
    64 bytes from 54.192.73.49: icmp_seq=0 ttl=245 time=3.281 ms
    64 bytes from 54.192.73.49: icmp_seq=1 ttl=245 time=4.472 ms
    64 bytes from 54.192.73.49: icmp_seq=2 ttl=245 time=3.471 ms
    64 bytes from 54.192.73.49: icmp_seq=3 ttl=245 time=4.519 ms
    64 bytes from 54.192.73.49: icmp_seq=4 ttl=245 time=2.512 ms

    --- doi5jqyokxnhv.cloudfront.net ping statistics ---
    5 packets transmitted, 5 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 2.512/3.651/4.519/0.761 ms
{{</highlight>}}
Old:
{{<highlight bash>}}
    ❯ ping -c 5 engjole.net
    PING engjole.net (167.99.97.195): 56 data bytes
    64 bytes from 167.99.97.195: icmp_seq=0 ttl=49 time=18.172 ms
    64 bytes from 167.99.97.195: icmp_seq=1 ttl=49 time=19.041 ms
    64 bytes from 167.99.97.195: icmp_seq=2 ttl=49 time=33.505 ms
    64 bytes from 167.99.97.195: icmp_seq=3 ttl=49 time=18.917 ms
    64 bytes from 167.99.97.195: icmp_seq=4 ttl=49 time=20.344 ms

    --- engjole.net ping statistics ---
    5 packets transmitted, 5 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 18.172/21.996/33.505/5.797 ms
{{</highlight>}}
The averages say it all: CloudFront is 7x faster than my old DO server, as one would hope and think 😉.

I think I've held onto doing websites on VPSes for so long because of the old-school ops feeling of nostalgia it brings, and I think my DO fleet will still have a place in my personal infrastructure. But for this site at least, CloudFront is going to rule for a good long while.
