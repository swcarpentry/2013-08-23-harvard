---
layout: lesson
root: ../..
title: Word Frequency
---

## An exercise in loops and words!

This exercise in counting the occurrences of words in a file serves to
illustrate some important iteration and data structure concepts in
Python.

I want to compare the number of times words occur in
Shakespeare's *[Hamlet](http://www.gutenberg.org/cache/epub/1524/pg1524.txt)*
with the number of times words occur in
Mark Twain's *[Adventures of Huckleberry Finn]
(http://www.gutenberg.org/cache/epub/76/pg76.txt)*
to see how the English language has changed over time.

The best way to follow along is to use an IPython shell.

## Download

We will use tools from the
[Requests](http://docs.python-requests.org/en/latest/) library,
included in both the
[Canopy](https://www.enthought.com/products/canopy/)
and
[Anaconda](https://store.continuum.io/cshop/anaconda/)
Python distributions,
to download the books in plain-text format.

First we need to tell Python that we are using the Requests library.

    import requests

Then we can use the `get` function from the Requests library to download
webpages and whatever other files are located at web addresses.

    hamlet_url = 'http://www.gutenberg.org/cache/epub/1524/pg1524.txt'
    hamlet_page = requests.get(hamlet_url)
    hamlet_text = hamplet_page.text


## Verify

What did we download? Let's check if there's data.

    len(hamlet_text)

Ok, so there's something there. How about we print a little bit.

    hamlet_text[:120]

Darn, I was hoping to have this as a list of lines. How can I get that?
Let's see what the Python documentation says about [splitting lines]
(http://docs.python.org/2/library/stdtypes.html#str.splitlines)

    hamlet_lines = hamlet_text.splitlines()
    hamlet_lines[:5]

Ok, now I can read the book more easily. Let's see how long the header text is
about Project Gutenberg and determine at which point the book starts.

    hamlet_lines[280:300]

It looks like line 290, or is that 291?

    hamlet_start = 290
    hamlet_lines[hamlet_start]

Let's see what's at the end.

    hamlet_lines[-10:]

Ok, there's only a few lines of legal text at the end.

    hamlet_end = len(hamlet_lines) - 7
    hamlet_lines[hamlet_end:]


## Loop

We've got lines, but how can we make words? This [split]
(http://docs.python.org/2/library/stdtypes.html#str.split) function looks
useful.

    hamlet_lines[hamlet_start].split()

Let's test looping through the words of a single line.

    for word in hamlet_lines[hamlet_start].split():
        print(word)

How can we count the occurrences as we iterate through the words? The [Counter]
(http://docs.python.org/2/library/collections.html#collections.Counter) class
would be good, but we'll use an ordinary [dictionary]
(http://docs.python.org/2/tutorial/datastructures.html#dictionaries) for
pedagogical reasons.

    counts = dict()
    for word in hamlet_lines[hamlet_start].split():
        counts[word] += 1

Oh no! (That's exactly the error that the Counter class prevents.)

    counts = dict()
    for word in hamlet_lines[hamlet_start].split():
        if word not in counts:
            counts[word] = 0
        counts[word] += 1
    counts

Lookin' good. Now we're ready to loop through the whole play.

    counts = dict()
    for line in hamlet_lines[hamlet_start:hamlet_end]:
        for word in line.split():
            if word not in counts:
                counts[word] = 0
            counts[word] += 1

## Analyze

What's the most frequent word?

    max(counts.values())

Ok, we know how often the most frequent word occurred, but what was it?

    reverse = [(count, word) for word, count in counts.items()]
    max(reverse)

Duh. What about the top 20 most common words?

    sorted(reverse)[:20]

No, those are single occurrances.

    sorted(reverse, reverse=True)[:20]

There we go.

## Fun for the Reader

Can you repeat this exercise for Huckleberry Finn and compare?
