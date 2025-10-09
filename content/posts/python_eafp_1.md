+++
draft = false
date = 2022-04-22T16:22:23-07:00
title = "Bool Mangling in Requests: Asking Forgiveness in Python, Pt 1"
description = "Asking Forgiveness In Python Pt 1"
slug = "Asking Forgiveness In Python Pt 1"
authors = ["Enny J. Frick"]
tags = ["coding", "python", "eafp", "coding idioms"]
categories = ["blog", "coding", "python", "eafp", "coding idioms"]
externalLink = ""
series = ["Asking Forgiveness in Python"]
+++

## The `if obj` Pattern

I recently ran into the following pattern in some code that implemented the very popular and ubiquitous (for good reason) [`requests`](https://docs.python-requests.org/en/latest/) library:

{{<highlight py>}}
import requests

r = requests.post(url="some.url.tld", headers=some_dict)

if r:
    do_some_stuff(r.json())
else:
    freak_out()
{{</highlight>}}

Now, this was weird for me to see. My initial interpretation of what this code was doing was checking for the existence of a `requests.Response` object from a `POST` request and if said object exists, do something with that response. In other words: let's make a `POST` request, and if everything is fine and dandy, it should return a `requests.Response` object we can process, and if not, it won't. So we should check for that object, and continue on if it exists.

## Methods and Properties of `requests.Response`

Seems okay in theory, but I think most users of the `requests` library are aware that there will be no `None` object returned in any circumstance, so this if check will always pass and we'll always `do_some_stuff()` and never `freak_out()` even if there's something wrong with the request. Ideally, we'd use the `requests.Response.raise_for_status()` method with a `try: except:` block to properly handle a bad request.

I made a comment about this during code review and decided to do some testing on Python 3.9 to check out this behavior a little more, using a purposefully unauthenticated and improperly formed request to an endpoint of [The Cat API](https://thecatapi.com) for testing:

{{<highlight py>}}
# test.py

import requests

def if_r():
<!-- markdownlint-disable-next-line -->
    r = requests.post("https://api.thecatapi.com/v1/images/upload")
    if r:
        print("if r")

def raise_for_stat():
<!-- markdownlint-disable-next-line -->
    r = requests.post("https://api.thecatapi.com/v1/images/upload")
    r.raise_for_status()

if __name__ == '__main__':
    if_r()
    raise_for_stat()
{{</highlight>}}

To my (slight) surprise, I did not get `"if r"` printed to `stdout`, only an `HTTPError` raised. I say "slight" because Python `if` is of course in reality not an existence check but a truthiness check; it just so happens that `bool(None)` evaluates to `False` so you can use `if` as an existence check...sometimes. I was most surprised that a `request.Response` object had a change in `bool` evaluation based on request status. This felt weird to me; the idiom in Python is to ask for forgiveness (handle exceptions) rather than check for validity, as part of being explicit rather than implicit, so it seemed weird you could use `requests` in a non-idiomatic way, especially given the ease of using `requests.Response.raise_for_status()`.[^1] I ended up looking at the [source code](https://docs.python-requests.org/en/latest/_modules/requests/models/#Response) for `requests.Response` and found something interesting:

{{<highlight py>}}
def __bool__(self):
    """Returns True if :attr:`status_code` is less than 400.

    This attribute checks if the status code of the response is between
    400 and 600 to see if there was a client error or a server error. If
    the status code, is between 200 and 400, this will return True. This
    is **not** a check to see if the response code is ``200 OK``.
    """
    return self.ok
{{</highlight>}}

So the `__bool__` special method is just a wrapper around the `requests.Response.ok` property, which shows up explicitly in the API documentation for `requests.Response`. `requests.Response.ok` probably just runs some checks on the `status_code` attr then, right?

{{<highlight py>}}
@property
def ok(self):
    """Returns True if :attr:`status_code` is less than 400, False if not.

    This attribute checks if the status code of the response is between
    400 and 600 to see if there was a client error or a server error. If
    the status code is between 200 and 400, this will return True. This
    is **not** a check to see if the response code is ``200 OK``.
    """
    try:
        self.raise_for_status()
    except HTTPError:
        return False
    return True
{{</highlight>}}

Nope! It just uses the good ol' `try: except:` control flow with `raise_for_status()` Considering this is what I was advocating to use, I was very pleasantly surprised to find this pattern in the source code.

## Why This Irritates Me

In my opinion, using `if r:` like this becomes a very roundabout way of avoiding exception handling _without avoiding exception handling at all_. To avoid using `try: except:`, you'd call `bool(some_response_object)` which looks at `some_response_object.ok` property...which uses `try: except:`. One of the reasons to avoid exception handling is the performance cost of throwing and handling an exception, but in this case you're going to throw and handle an exception no matter what since that's how `self.__bool__`/`self.ok` is implemented here. Plus, exceptions aren't even that expensive in Python to begin with, so you're left with readability and complexity claims. Sure, there are definitely cases where readability will suffer due to extended `try: except:` blocks, especially if these are located within other control flows, but I'd argue `if r:` isn't that readable to begin with, since, if you don't know that `self.__bool__` in this case just shadows `self.ok`, which I personally did not until I looked at the source code, it can be ambiguous on how the truthiness of the `requests.Response` object is determined, and in which cases the `if` block will evaluate as `True`.

I am also just not sure in general how much I like the fact that the truthiness of a `requests.Response` instance is dependent on an external system. Perhaps it's my slight affinity for functional programming and formal logic (Haskell was the first language I ever touched, fun fact!) that makes me a little adverse to this; it just feels off to me to rely on either internal or external state for truth value testing in this way, even though epistemically I hold that truthiness can be evaluated in terms of external relations to the world in the case of empirical statements (I will stop here in regards to epistemology since it would take several book-length works for me to satisfactorily talk about it). The Python People™ definitely don't share quite the same reservations; if `object.__bool__(self)` is undefined `__len__()` will be called instead and non-zero values will be considered true, meaning there is a strong relationship between state and truthiness (if `__len__()` is undefined `__bool__()` then becomes an existence check). Still, I do think relying on properties or attrs of objects for control flow like this, rather than how truthiness is implemented, is better for readability and explicitness.

## Some Final Thoughts

Overall, I find `requests.Response.__bool__()` to be an example of what I call _bool mangling_, a not-very-explicit-or-clear-on-first-glance implementation of `__bool__` that may be intended as sugar but doesn't really do much for readability, at least in my opinion. Instead of using `if r:`, we should ask for forgiveness, use `r.raise_for_status()`, and properly handle HTTP errors as they occur when we're making requests, rather than just suppressing them and examining the validity of a response using truth value testing. It's better to clearly communicate what the code is doing (e.g. processing data only on a successful request) than not; as [_The Zen of Python_](https://peps.python.org/pep-0020/) states, "Explicit is better than implicit." And, when we _do_ need to use an `if` check in this case, in the times when the `r.raise_for_status()` pattern becomes _too_ explicit ("Readability counts."), we should use `if r.ok:` instead, to clearly communicate what we're doing.

[^1]: Funnily enough I later realized that in the docs for `requests.Response.json()` the Requests maintainers explictly say to use `raise_for_status()` instead of ugly if checks, since `if r.json()` has slightly different behavior from its parent class.
