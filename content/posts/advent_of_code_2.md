+++
draft = false
date = 2021-12-02T12:00:00-08:00
title = "Advent of Code 2021: Day 2"
description = "Advent of Code '21 Day 2"
slug = "Advent of Code 21 Day 2"
authors = ["Enny J. Frick"]
tags = ["code", "challenge", "advent of code", "advent of code 2021"]
categories = ["advent of code", "advent of code 2021", "blog", "coding"]
externalLink = ""
series = ["Advent of Code 2021"]
+++

## Nothing Snarky to Say This Time

But wait, isn't that saying something snarky?

## Day 2 Spoilers

### The Problem

This one is a bit of a doozy...

Given a list of strings of that match the (Python) regex ```"(forward|up|down)[\s][0-9]"gm```[^1], calculate the final coordinate pair (starting from 0, 0) at the end of the list of strings, where ```forward``` means an increase in the _x_ direction and ```up``` and ```down``` mean a decrease and increase in the _y_ direction, respectively. Additionally, find the product of _x_ and _y_.

[^1]: This is so gross I hate regex sometimes.

### The Solution

Again, I think we're better served by the example given by Advent of Code (which I'll reword slightly anyway), and not my gross, detached, formal-logic-style rewording of the problem:
{{<highlight text>}}
Given the following instructions:

forward 5
down 5
forward 8
up 3
down 8
forward 2

You start from [0, 0]. The steps above would then modify your position as follows:

forward 5 adds 5 to your x, a total of 5.
down 5 adds 5 to your y, resulting in a value of 5.
forward 8 adds 8 to your x position, a total of 13.
up 3 decreases your y by 3, resulting in a value of 2.
down 8 adds 8 to your y, resulting in a value of 10.
forward 2 adds 2 to your x, a total of 15.

After following these instructions, you would have a x of 15 and a y of 10. (Multiplying these together produces 150.)
{{</highlight>}}
There we go! Much easier to understand. Thank god I'm not writing these problems, right? It'd require some moral philosophy background just to understand the first sentence.

First part, as always, is turn the input webpage into something useful:
{{<highlight py>}}
def get_input_data(url: str) -> list[str]:
    return [item for item in requests.get(url, cookies={"session": SESSION_ID}).text.split("\n")]
{{</highlight>}}
Hey look at that, a one-liner! Aren't I clever. I go back and forth of whether I like naming list comps something understandable before returning them or if I just want to return the comp and let the function name do the semantic work (especially when it has type annotations). This time laziness won out  ¯\\_(ツ)_/¯.

This is once again a fairly straightforward problem. You just need to track the _x_ and _y_ position somehow, and have a case/match/if-else if check for each word and deconstruct each line. I could probably have done this with the new structural pattern matching syntax in Python 3.10 but I couldn't be assed that much. Again, laziness won out.
{{<highlight py>}}
def get_horizontal_and_vertical_position(command_list: list[str]) -> list[int]:
    final_position = [0, 0]
    for command in command_list:
            if command.startswith("down"):
                final_position[1] += int(command[-1])
            elif command.startswith("up"):
                final_position[1] -= int(command[-1])
            elif command.startswith("forward"):
                final_position[0] += int(command[-1])

    return final_position
{{</highlight>}}
I probably should have set _x_ and _y_ to separate variables rather than operate on list elements the whole time. It would have been a little easier to read, especially since I could just create a list at function return with those values or even just return them as a tuple or even just do the multiplication step now instead of as an f-string during final output. That said, I feel like coupling them together like this reflects their relationship better, and returning the final position just felt cleaner to me; I'd rather write a little broader code that could theoretically be reused and then further manipulate the output rather than hyperspecific code that would have to get modified for a more general use case. Yeah, this is Advent of Code and I'll probably never use this code ever again, but _writing production-like code is always a good practice_.

This passed successfully post-element multiplication.

### The Problem, Part 2

Given a list of strings of that match the (Python) regex ```"(forward|up|down)[\s][0-9]"gm```[, calculate the final coordinate triple (starting from 0, 0, 0) at the end of the list of strings, where ```forward n``` means an increase in the _x_ direction and an increase in the _y_ direction by _n_ * _z_ and ```up n``` and ```down n``` mean a decrease and increase in the _z_ direction, respectively. Additionally, find the product of _x_ and _y_.

### The Solution, Part 2

Again, pretty straightforward: just track _x_, _y_, and _z_ and do an if-check to match the conditions given:
{{<highlight py>}}
def get_hori_vert_and_aim(command_list: list[str]) -> list[int]:
    final_position = [0,0,0]
    for command in command_list:
        if command.startswith("down"):
            final_position[2] += int(command[-1])
        elif command.startswith("up"):
            final_position[2] -= int(command[-1])
        elif command.startswith("forward"):
            final_position[0] += int(command[-1])
            final_position[1] += (int(command[-1]) * final_position[2])

    return final_position
{{</highlight>}}
Again, could have done a ```match``` instead. No biggie. I'm sure I could have also done this without the ```startswith``` syntax during the iteration and instead convert values in-place in the input list and then add them somehow to the final position. I do think loops like this are a little easier to read than some clever ```map``` stuff...but clever ```map``` stuff has its place.

## Conclusion

Nothing to write home about, a nice lunchtime/coffee time brain exercise to focus on this instead of holiday cards.

[All code in full is in GitHub](https://github.com/ENG-Jole/advent-of-code-2021)
