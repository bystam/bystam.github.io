---
layout: post
title: "Models and exceptions"
date: 2030-11-30 11:00:44 +0200
categories: takes
---

# https://www.desmos.com/calculator/28vttpsipw

As anybody sufficiently interested in software development, I spend a decent amount of time thinking about what makes certain software designs either good or bad. In this instance, more specificaly, what makes them easy to work with or not. More specifically, I am constantly chasing new and better ways to express in natural language if there are any similarities, and ways to describe what makes for a good design as opposed to a bad one.

I have become friends with one such way of thinking about them - and I call it "models vs. exceptions".

## What is a model?

Whenever I intend to build something, be it a module or a feature, or even significantly extending something existing - I sometimes force myself to try and "write two or three sentences that capture the essence of the software". Something written in plain English that can give the reader the fundamental idea behind the solution. The goal is to have it expressed in such a way that it lets readers extrapolate from it and understand downstream effects of said description without having those necessarily be explicitly written down.

I call this description, for lack of a better phrase, "the model". By "model", I don't necessarily mean database tables, or domain types in the code - although those can absolutely be part of it. I mean even more broadly - true statements that paint a picture of the most important parts of the system or feature.

What are some examples? For instance, here would be one that describes commits in Git:

> Git commits are immutable snapshots that each contain an entire (logical) copy of the file tree. Each commit has one or more parents, creating a directed acyclic graph that forms the commit history. Commits are identified by a SHA-1 hash derived from its content and metadata.

This is a pretty concise summary of the most important parts of git commits. It provides answers to various questions about git by deriving from the model description:

**_Q: How come commits get a new hash when you ammend it?_**

A: Since the hash is based on the contents of the commit - changing its content forcibly changes the identifier

**_Q: How come checking out an old commit, even a super old one, is nearly instant?_**

A: Every commit contains a complete copy of the file tree, which means that it only needs to jump to that specific commit to load the complete state of all files.

## What is an exception?

Here, we are not talking about the kind of thing people love to throw in Java. Exceptions are exceptions to the model. Something that is NOT implicit from the original description, but rather some edge case where the system behaves outside of its model behavior.

The quintessential exception in code would look like this:

```kotlin
fun someOperation() {
    if (isExceptionCondition) {
        doSomethingElse()
        return
    }

    workAccordingToModelDescription()
}
```

In plain English, exceptions are the things you would state after the model description by opening with "except..." and then enumerating the cases where things do not behave as expected.

It is tempting to think of exceptions as "the ugly hacks" that one has to bolt onto a beautiful system because Carl from marketing promised features to a client before talking to the tech team. I will argue that they are usually integral to a well behaved, comprehensible system.

## When does the model fit?

I aim to end with describing a soft heuristic for how to know when you have a model that fits just right for the system you intend to design. But before I do that, I want to tell you about the analogy I use.

### The analogy

Why I have settled on calling them "models" is because the picture in my head for them comes from mathematics (or machine learning if you will).

When I paint it, I like to think of "requirements" or "feature requests" as being points on a graph.

![Requirements](/assets/model-graph-dots.png)

These points represent examples of how the system behaves. You can think of them as user stories, or similar.

How would you go about creating a description of all of these points? Of course, we could simply just create a long list of "when x is 3, y is 4" etc. But a more succinct one, that captures more than just the examples, would be to create an equation that intersects all of them. Because simply saying `y = 2x + 1` is much more compact, and has greater coverage, than a long list of points.

This equation is the model.

#### An awkward feature request appears

Now

### Over- and under-fitting

### Back to software

The software equivalent of simply giving someon a list of 10 points, as opposed to the equation, would be when you as the developer has no way of expressing how the system works without starting to list all the if-statements in the code.
