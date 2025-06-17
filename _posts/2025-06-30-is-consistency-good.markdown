---
layout: post
title: "Is perfect consistency really good?"
date: 2025-06-30 11:00:44 +0200
categories: takes
---

Like all developers, I often find myself in a debate about whether we should introduce automatic formatting and linting tooling to help extinguish inconsistencies in the codebase.

Anybody who has written modern Javascript or Typescript knows that the web is built by people treating their `prettier.config.mjs`-files as if they were beautiful Bonsai trees. Programming without proper linting plugins to handle the preferred import order and formatting is considered as reckless as dumping radioactive waste in a local lake where kids go swimming in the summer.

And if you are a typical "backend developer", surely you agree there are **very good reasons** for why you cannot let a "controller" have direct access to a "repository" without first crossing _the service layer_? Obviously, if some parts of your system are large enough to warrant a certain amount of architectural layers, then said layers must be applied everywhere else too? Thank God that we can enforce it with things like [ArchUnit](https://www.archunit.org/).

All of this to achieve a harmonious level of consistency across the codebase. Because said consistency **obviously** comes with improved readability.

## ...or does it?

As with anything related to software, there is hardly any science to back up any claims of one style being more readable than another. In fact, "best practices" in general are mostly held together by the figment of our collective imagination (although that is a blog post for another day).

How certain are we that consistency actually does improve our ability to comprehend what is written? I think a lot about what works for myself when I configure my editor. My preferred color schemes make code look like a Christmas tree; I want as much discrepancy between different syntactic and semantic elements as possible. Function parameters, local variables and class fields all have different colors? Love it. Global variables are in cursive? Amazing. Methods/functions are blue while member variables are white? That's the stuff.

There is something about things being visually different that [helps my brain subconsciously](https://en.wikipedia.org/wiki/Thinking,_Fast_and_Slow) make sense of it without me needing to explicitly read and think hard about the tokens. Giving different constructs of the source code a certain level of [salience](<https://en.wikipedia.org/wiki/Salience_(neuroscience)>) helps me identify important things without contemplating every identifier or paranthesis.

Here's how the creator of a [particular font for dyslexic people](https://dyslexiefont.com/en/) describes what makes it simpler to read:

> Designed by a dyslexic, the font focuses on legibility by enhancing visual distinction between characters, reducing reading complexity...

"Visual distinction" stands out as an interesting property of the font. That such a thing would help readability does feel rather intuitive.

## Could inconsistencies be... good, actually?

Do you ever feel like you're navigating a codebase where all the modules, classes and packages are so consistent that you struggle to even tell them apart? If every single piece of CRUD comes in identically layered `web -> service -> repository -> domain model` shapes, then the sheer weight of architectural harmony suddenly drowns out the discrepancies in the business logic.

But what if one didn't need to have a single blueprint for all modules? What if every module could be slightly different, where the architectural sophistication grows from a necessity to accommodate for the unique requiements and complexity of the domain?

Could it possibly be quite helpful to open a package and notice that _"this package only has an endpoints and a model file - it must be really simple"_?

And with formatting - if certain files or modules turned out to look a little bit different - is it possible that it serves as contextual clues for the brain to tell things apart? Could certain styles help your brain identify the author of the code (your colleague), letting you more easily prime your brain to read the entire code the way they typically write it?

You could think of it as micro-dosing inconsistencies if you will. I'd like to think of them as the ability to navigate neighborhoods of identical houses based on what cars people have in the driveway, or how they have decided to decorate their front lawn.

## Good use of consistency

As any [good](https://www.oxfordlearnersdictionaries.com/definition/english/bad_1) blog poster, I obviously make straw man arguments that support a rather weak take. The obvious benefit of some kind of auto-formatter is that **formatting indeed does matter**. Formatting is part of the puzzle when it comes to making code comprehensible. For instance:

- Lumping cohesive lines together with separating empty lines can create visible "sections" of a function body
- Leveraging indentation to indicate control flow (even if your curly brace-language doesn't require it)
- Placing longer sequences of parameter arguments on separate lines to avoid them disappearing from the screen

And if it turns out that you have a team or an organization where people simply do not spend a lot of time thinking or worrying about formatting, then an auto formatter might absolutely outperform the median outcome without it.

Similarly - if you have a server application where different packages of similar size and complexity have completely different styles "just because", then that obviously can hurt one's ability to understand it. Words have meaning (or should at least), so when someone actually does decide to call something a "repository" or a "use case", it is indeed useful if the name is a clue towards what it actually contains. And on the flip side, if every class is a "service", then the word "service" has no meaning.

## Where are you even going with this?

I don't intend to suggest that these small inconsistencies are obviously and provably better.

I mostly for myself wonder whether there is actually a cutoff point where the use of all these automated tools, these rules about architecture and about naming, where they start working against their stated goal. And whether we are too blind to see it, because setting up the perfect automated code analysis pipieline just feels so good.

What do you think?
