---
layout: post
title:  "Implicit properties of your data model"
date:   2024-03-20 20:49:49 +0100
categories: takes
---
I recently listened to an interview of the author of the famous [“The Grug Brained Developer”](https://grugbrain.dev) blog post, Carson Gross. In that interview, he reiterated a point that he also made in the original text - the quote being:

> one day code base understandable and grug can get work done, everything good!
> 
> next day impossible: complexity demon spirit has entered code and very dangerous situation!

The "next day impossible" is not only a funny expression. In the interview he talked of the experience of seeing a system grow beyond ones ability to comprehend it in just a matter of **weeks**.

That a simple system **slowly** evolves into a complex one through incremental additions of features, bug fixes and tweaks is a mystery to no one. But what are the kinds of changes that can break one's mental model of the workings of a system within just a couple of weeks? I will argue that I have an example of such a thing.

### Implicit data modeling
One thing we constantly do when programming is creating data models - be it in APIs, in the database or elsewhere. This is the bread and butter of working as a software developer.

Let’s give a simplified example of a real-world scenario I have experienced (more than once!). Imagine our system has a `User` type stored in the database with an id, name and email. Imagine we don’t store any password, because the user instead logs in by getting a one-time-password sent to their email.

One interesting aspect of models like these are the things they convey that are not explicitly written down or reflected in some data contract - but are rather implicitly assumed. One common case with all `User`-types like these is that they were all created when somebody decided to “sign up” to our platform. That implies that all the entries in that database are customers that signed up for our service. From that, the following assumptions are probably safe to make about those people:
* They have gone through some kind of signup or onboarding flow
* They all had to accept some kind of “terms and conditions” as part of signing up
* They are **aware** that they are in fact one of our customers


### A wild feature appears
Now let us introduce a feature request. The growth side of the company believes that more people would sign up and start paying for our product if they could get a nice, free, demo of the app. Therefore, they would like to send marketing emails with a link where people can get straight into the demo mode.

“Easy!” says some developer. “If we simply create `User` entries for their emails, they will be able to log in. And to keep track of whether they should see the demo or the real experience we give the user a new column called `type` which is either `real` or `demo`. Let’s ship it!”

Even in a real world scenario, this kind of change to a model can often rapidly be built and shipped with not that much code. And at a first glance, the addition was totally backwards compatible, right? All the existing `User`s presumably had their `type` value set to `real`, and thus we wouldn’t corrupt any old users.

Except - what about those implicit assumptions about the model? Suddenly we have a pool of users that no longer match our existing mental model. What if:
* Some team works with cross-referencing our own data with data found in external sources and uses “Do we have a `User` with that email” as a proxy for “is that email a customer of ours?” They will now have tonnes of false positives.
* Some team uses the changes of the user table to track growth of the company customer base? Suddenly it might look like you grew way more than anticipated. If you’re lucky you’ll catch it - but if the change is too subtle then you might show the wrong numbers to stakeholders.
* This size of the growth hack campaign even outnumbers the size of your customer base? What if you had 10k users and we sent 20k emails? Suddenly your user table grew by 200% and what used to be the only type of user is now in the minority. With an addition of this size, a small problem can turn into a large one if the work required to restore it is proportional to the amount of “demo users”.

Adding a completely new way of creating `User` has potentially broken the mental model a lot of your colleagues had. In a sense, the change turned out not to be backwards compatible at all. To adjust for it, lots of other integration points might be forced to be updated with an extra `where type = ‘real’` filter to restore the previous behavior. If you are a small company and you quickly identified all 3-5 of those integration points - great! If you are a bigger company, and you actually are not even aware of the existence of some teams who depended on the old behavior - uh oh…! Chances are that they won't know of your change until much later when they're starting to see "weird errors coming from the users API". Maybe the change in this crucial integration point has done some form of “irreversible damage”, such as accidentally sending emails to non-customers.

Not to mention the fact that future feature additions now have to take demo users into account. Additions that might have fit naturally into the old version of the model might suddenly feel awkward in the new one.

### What else could have been done?
The original bet was that “if people could try our platform out in a demo mode, then maybe they would be impressed enough to buy it”. Another approach here would be to identify a small, isolated subset of your product and simply copy-paste it into a stateless version that can be used without logging in at all. In other words closer to something like an “interactive mockup” that looks and feels like the real deal, but is more of a temporary playground. If it turns out users would like to keep their playground-work after signing up, you could potentially “import” that data into the real model after a successful signup.

This might mean doing a little bit more work upfront, and it might force you to keep two UIs up to date for now. But the upside is real. This kind of solution is way less likely to “leak” to the rest of the system and teams - and if it turns out that the demo mode turned out to not work, then simply deleting this copy is dirt cheap. In contrast, cleaning up the demo users could be expensive if their presence has had ripple effects in the system.


### In summary
In my experience, it sometimes takes very little code to make a rather drastic change in a model. This category of changes, which breaks the mental picture people might have of the model, can very rapidly induce a massive amount of complexity into a system. All with a very modest change.

You might think that the real problem here is that people shouldn’t make implicit assumptions about data models at all. There is some truth to that, but it’s not a particularly useful opinion in a fast moving environment where everyone is doing their best. In the real world, people will make these kinds of assumptions all the time, either consciously or subconsciously. A lot of the time, such an assumption will work well enough and let a team build something useful for your customers. If you are maintainer of the part of the system that holds the `User` table, it is much cheaper for your team to think twice before shipping an expansion of your model, than it is for **everyone** else at the company to be paranoid about what you might do.

**_Do with that what you will._**
